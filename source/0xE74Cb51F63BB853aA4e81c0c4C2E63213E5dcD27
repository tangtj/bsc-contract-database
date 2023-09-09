// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function burn(uint256 amount) external;
    function mint(address recipient, uint256 amount) external;
}

contract PolanixMain {
    mapping (address => User) public users;
    mapping (uint => address) public userIdsAddress;
    mapping (uint => Pump) public pumps;
    mapping (address => uint) public usersActivePumps;
    mapping (uint => Exchange) public exchanges;
    mapping (address => uint) public lastExchangedAt;
    mapping (address => uint) public exchangesAmount;
    mapping (uint8 => address) public feeAddress;
    mapping (uint8 => uint) public referralsDeposit;
    mapping (uint8 => uint) public referralsClaim;
    mapping (uint8 => uint) public percentages;
    mapping (address => Allowed) public allowedExchangers;

    address public ownerAddress;
    address public tokenAddress;

    uint public nextUserId;
    uint public nextPumpId;
    uint public nextExchangeId;
    uint public usersCount;
    uint public tokenPrice;
    uint public tokenPriceAdd;
    uint public maxPumpAmount;
    uint public minPumpAmount;
    uint public minReinvestAmount;
    uint public pumpPeriod;
    uint public pumpsCount;
    uint public depositsCount;
    uint public minExchangePercent;
    uint public maxExchangePercent;
    uint8 public activeReferralLines;
    uint8 public minReferralLines;
    uint public feePercent1;
    uint public feePercent2;
    uint public feePercent3;
    uint public feePercent4;
    uint public feePercent5;
    uint public feePercent6;

    event UserAdded(address indexed user, uint indexed userId, address indexed referral, uint createdAt);
    event PumpCreated(address indexed user, uint indexed pumpId, uint amount, uint dailyPercent, uint createdAt);
    event ReferralRewardTransferred(address indexed user, address indexed referral, uint indexed pumpId, uint8 line, uint amount, uint amountReward, bool deposited);
    event PumpRewardTransferred(address indexed user, uint indexed pumpId, uint dailyPercent, uint daysReward, uint amountReward, uint withdrawnAt);
    event PumpDeposited(address indexed user, uint indexed pumpId, uint dailyPercent, uint amount, uint amountDeposited, uint reinvestCount, uint amountExchangeDeposited, uint amountExchangeLimit, uint amountReinvested, bool tokens);
    event ExchangeTransferred(address indexed user, uint indexed exchangeId, uint price, uint amountReceived, uint amountSent, uint amountReturn, uint exchangeLimit, uint userExchangedAmount, uint exchangedAt, bool timer);
    event ExchangeContractsTransferred(address indexed user, uint price, uint amountReceived, uint amountSent, uint exchangeLimit, uint contractExchangedAmount, uint exchangedAt);
    event UserReferralEarned(address indexed user, uint amount, uint amountEarnings, uint amountEarningsTokens, uint amountExchangesTokens, bool tokens, bool fee);
    event UpdatedUint(uint8 indexed id, uint value, uint updated);
    event UpdatedAddress(uint8 indexed id, address value, uint updated);

    struct User {
        uint id;
        address referral;
        uint referralEarnings; //BNB
        uint referralEarningsTokens; //in BNB
        uint referralExchangesTokens; //in BNB
        uint createdAt;
    }

    struct Pump {
        uint id;
        address user;
        uint amountDeposited; //BNB
        uint amountWithdrawn; //TOKEN
        uint amountExchanges; //Exchanges BNB
        uint reinvestCount;
        uint amountExchangeDeposited;
        uint amountExchangeLimit;
        uint amountReinvested; //Reinvest BNB
        uint dailyPercent;
        uint withdrawnDaysCount;
        uint createdAt;
    }

    struct Exchange {
        uint id;
        address user;
        uint price;
        uint amountReceived; //TOKEN
        uint amountSent; //BNB
        uint exchangedAt;
    }

    struct Allowed {
        uint limit;
        uint exchanged;
        bool allowed;
    }

    constructor() {
        nextUserId = 1;
        nextPumpId = 1;
        nextExchangeId = 1;
        usersCount = 0;
        tokenPrice = 1e16; // 0.01 BNB
        tokenPriceAdd = 5 * 1e13; // 5 * 1e13 - 0.00005 BNB
        maxPumpAmount = 15 * 1e18; // 15 BNB
        minPumpAmount = 15 * 1e16; // 0.15 BNB
        minReinvestAmount = 1e16; // 0.01 BNB
        pumpPeriod = 1 days;
        pumpsCount = 0;
        depositsCount = 0;
        minExchangePercent = 10; // 0% - 30%
        maxExchangePercent = 30; // 30% - 100%
        activeReferralLines = 10; // 10 lines
        minReferralLines = 2; // 2 lines
        feePercent1 = 1000; // 10%
        feePercent2 = 1000; // 10%
        feePercent3 = 500; // 5%
        feePercent4 = 250; // 2.5%
        feePercent5 = 250; // 2.5%
        feePercent6 = 250; // 2.5%

        referralsDeposit[1] = 1500; //15%
        referralsDeposit[2] = 700; //7%
        referralsDeposit[3] = 300; //3%
        referralsDeposit[4] = 200; //2%
        referralsDeposit[5] = 100; //1%
        referralsDeposit[6] = 50; //0.5%
        referralsDeposit[7] = 25; //0.25%

        referralsClaim[1] = 200; //2%
        referralsClaim[2] = 100; //1%
        referralsClaim[3] = 50; //0.5%
        referralsClaim[4] = 50; //0.5%
        referralsClaim[5] = 50; //0.5%
        referralsClaim[6] = 25; //0.25%
        referralsClaim[7] = 25; //0.25%

        percentages[1] = 50; //0.5%
        percentages[2] = 75; //0.75%
        percentages[3] = 100; //1%
        percentages[4] = 125; //1.25%
        percentages[5] = 150; //1.5%

        ownerAddress = msg.sender;
        tokenAddress = address(0xB435d5ED9B819d5770452876256Eb960C4d4E20b);

        feeAddress[1] = address(0xf63475eE64a5A62C254D6a3E3cE58CE840837Bf5); // 10%
        feeAddress[2] = address(0x91F0C67966592Ad4BD07d3a5D2872d88FD721e35); // 10%
        feeAddress[3] = address(ownerAddress); //exchange fee
        feeAddress[4] = address(0x192653fB7bDDe176716f89B833099024Ed8276f4); //claim fee
        feeAddress[5] = address(0x3395D5A58F5527E4b0fb0002B30b7d3bd2454EF4); //claim fee
        feeAddress[6] = address(0); //leaders

        User memory user1 = User(nextUserId, address(0), 0, 0, 0, block.timestamp);
        users[ownerAddress] = user1;
        userIdsAddress[nextUserId] = ownerAddress;
        emit UserAdded(ownerAddress, user1.id, user1.referral, user1.createdAt);

        usersCount++;
        nextUserId++;
    }

     modifier onlyOwner() { 
        require(msg.sender == ownerAddress, "only owner"); 
        _; 
    }

    //check on smart contract
    function isContract(address minter) internal view returns (bool) {
        uint32 size;
        assembly {
            size := extcodesize(minter)
        }
        return (size > 0);
    }

    //check transfer on smart contract
    function _transferBnb(address wallet, uint amount) internal returns(bool) {
        if (wallet == address(0) || amount == 0) {
            return false;
        }

        if (isContract(wallet)) {
            (bool sent, ) = address(wallet).call{value:amount}("");
            return sent;
        } else {
            payable(wallet).transfer(amount);
            return true;
        }
    }

    function balanceTokens(address token) public view returns (uint balance){
        if (token == address(0)) {
            return address(this).balance;
        } else {
            return IERC20(token).balanceOf(address(this));
        }
    }

    function withdrawTokens(address token, uint value) public {
        _withdrawTokens(token, value);
    }

    function _withdrawTokens(address token, uint value) private onlyOwner() {
        if (token != address(0)) {
            if (value > 0) {
                IERC20(token).transfer(ownerAddress, value);
            } else {
                IERC20(token).transfer(ownerAddress, IERC20(token).balanceOf(address(this)));
            }
        }
    }

    function _updateExchanger(address exchanger, uint limit, bool allowed) private onlyOwner() {
        if (exchanger != address(0)) {
            Allowed memory allowedExchanger = Allowed(limit, 0, allowed);
            allowedExchangers[exchanger] = allowedExchanger;
        }
    }

    function updateAddress(uint8 id, address wallet) public {
       _updateAddress(id, wallet);
    }

    function _updateAddress(uint8 id, address wallet) private onlyOwner() {
        if (id > 0) {
            feeAddress[id] = wallet;
        }
    }

    function updateSettings(uint8 id, uint value) public {
         _updateSettings(id, value);
    }

    function _updateSettings(uint8 id, uint value) private onlyOwner() {
        if (id == 0 && value <= 1e16) {
            tokenPriceAdd = value;
        } else if (id == 1 && value <= 50 && value <= maxExchangePercent) { //0%-50%
            minExchangePercent = value;
        } else if (id == 2 && value >= 25 && value <= 100 && value >= minExchangePercent) { //25%-100%
            maxExchangePercent = value;
        } else if (id == 3 && value >= 1e16 && value <= maxPumpAmount) {
            minReinvestAmount = value;
        } else if (id == 4 && value >= 1e16 && value <= maxPumpAmount) {
            minPumpAmount = value;
        } else if (id == 5 && value >= minPumpAmount) {
            maxPumpAmount = value;
        } else if (id == 6 && value >= 2 && value <= 20) {
            minReferralLines = uint8(value);
        } else if (id == 7 && value >= 2 && value <= 20) {
            activeReferralLines = uint8(value);
        } else if (id == 8 && value >= 100 && value <= 2000) {
            feePercent1 = value;
        } else if (id == 9 && value >= 100 && value <= 2000) {
            feePercent2 = value;
        } else if (id == 10 && value >= 100 && value <= 2000) {
            feePercent3 = value;
        } else if (id == 11 && value >= 100 && value <= 2000) {
            feePercent4 = value;
        } else if (id == 12 && value >= 100 && value <= 2000) {
            feePercent5 = value;
        } else if (id == 13 && value >= 100 && value <= 2000) {
            feePercent6 = value;
        } else if (id >= 14 && id <= 20 && value <= 2000) {
            referralsDeposit[id - 13] = value;
        } else if (id >= 21 && id <= 27 && value <= 2000) {
            referralsClaim[id - 20] = value;
        } else if (id >= 28 && id <= 32 && value >= 10 && value <= 500) {
            percentages[id - 27] = value;
        } else {
            revert("invalid update setting");
        }
    }

    fallback() external payable {
        deposit();
    }

    receive() external payable {
       deposit();
    }

    function registration() public {
        _registration(msg.sender, address(0));
    }

    function registration(address referral) public {
        _registration(msg.sender, referral);
    }

    function _registration(address userAddress, address referralAddress) private {
        require(!isContract(msg.sender), "only user wallet");
        require(userAddress != address(0), "user address require");
        require(users[userAddress].id == 0, "user exists");

        if (referralAddress == address(0) || users[referralAddress].id == 0) {
            referralAddress = userIdsAddress[1]; //admin user
        }

        User memory user = User(nextUserId, referralAddress, 0, 0, 0, block.timestamp);
        users[userAddress] = user;
        userIdsAddress[nextUserId] = userAddress;
        emit UserAdded(userAddress, user.id, user.referral, user.createdAt);

        usersCount++;
        nextUserId++;
    }

    function isUserExists(address user) public view returns (bool exists) {
        return (users[user].id != 0);
    }

    function getUser(address user) public view returns (uint id, address referral, uint referralEarnings, uint referralEarningsTokens, uint referralExchangesTokens, uint createdAt) {
        return (users[user].id, users[user].referral, users[user].referralEarnings, users[user].referralEarningsTokens, users[user].referralExchangesTokens, users[user].createdAt);
    }

    function getPump(uint pumpId) public view returns(address user, uint dailyPercent, uint amountDeposited, uint amountWithdrawn, uint withdrawnDaysCount, uint createdAt) {
        require(pumps[pumpId].id != 0, "pump not found");

        return (pumps[pumpId].user, pumps[pumpId].dailyPercent, pumps[pumpId].amountDeposited, pumps[pumpId].amountWithdrawn, pumps[pumpId].withdrawnDaysCount, pumps[pumpId].createdAt);
    }

    function getPumpLimits(uint pumpId) public view returns(uint amountExchanges, uint reinvesstCount, uint amountExchangeDeposited, uint amountExchangeLimit, uint amountReinvested) {
        require(pumps[pumpId].id != 0, "pump not found");

        return (pumps[pumpId].amountExchanges, pumps[pumpId].reinvestCount, pumps[pumpId].amountExchangeDeposited, pumps[pumpId].amountExchangeLimit, pumps[pumpId].amountReinvested);
    }

    function deposit() public payable {
        if (users[msg.sender].id == 0) {
            _registration(msg.sender, address(0));
        }

        if (usersActivePumps[msg.sender] == 0) {
            require(msg.value >= minPumpAmount, "invalid value");
            _createPump(msg.sender, msg.value);
        } else {
            require(msg.value >= minReinvestAmount, "invalid value");
            require(pumps[usersActivePumps[msg.sender]].user == msg.sender, "pump user different");

            _withdrawRewardsPump(usersActivePumps[msg.sender], false);

            _depositPump(usersActivePumps[msg.sender], msg.value);
        }
    }

    function deposit(address referral) public payable {
        require(msg.value >= minPumpAmount, "invalid value");
        require(usersActivePumps[msg.sender] == 0, "pump already exists");
        if (users[msg.sender].id == 0) {
            _registration(msg.sender, referral);
        }

        _createPump(msg.sender, msg.value);
    }

    function deposit(uint pumpId) public payable {
        require(msg.value >= minReinvestAmount, "invalid value");
        require(pumps[pumpId].user == msg.sender, "pump invalid");
        require(usersActivePumps[msg.sender] == pumpId, "active pump invalid");

        _withdrawRewardsPump(pumpId, false);

        _depositPump(pumpId, msg.value);
    }

    function claim() public {
        withdraw();
    }

    function withdraw() public {
        require(usersActivePumps[msg.sender] > 0, "active pump invalid");
        require(pumps[usersActivePumps[msg.sender]].user == msg.sender, "pump user different");
        require(pumps[usersActivePumps[msg.sender]].id != 0, "pump not found");
        require(users[pumps[usersActivePumps[msg.sender]].user].id != 0, "user not found");

        _withdrawRewardsPump(usersActivePumps[msg.sender], true);
    }

    function withdraw(uint pumpId) public {
        require(pumps[pumpId].id != 0, "pump not found");
        if (msg.sender != ownerAddress) {
            require(pumps[pumpId].user == msg.sender, "pump user different");
            require(usersActivePumps[msg.sender] == pumpId, "active pump invalid");
        }
        require(users[pumps[pumpId].user].id != 0, "user not found");

        _withdrawRewardsPump(pumpId, true);
    }

    function exchangeContractsTokens(address exchanger, uint amount, address token)
    public 
    {
        require(msg.sender == tokenAddress, "sender invalid");
        require(amount > 0, "amount invalid");
        require(exchanger != address(0), "exchanger invalid");
        require(allowedExchangers[exchanger].allowed, "exchanger address no allowed");

        if (token == tokenAddress) {
            require(IERC20(tokenAddress).balanceOf(exchanger) >= amount, "tokens not enough");
            require(IERC20(tokenAddress).transferFrom(exchanger, address(this), amount), "transfer error");
            IERC20(tokenAddress).burn(amount);

            _exchangeContracts(exchanger, amount);
        } else {
            require(IERC20(tokenAddress).transferFrom(exchanger, address(this), amount));
        }
    }

    function _exchangeContracts(address exchanger, uint amount) private {
        require(tokenPrice > 0, "token price invalid");
        require(allowedExchangers[exchanger].allowed, "exchanger address no allowed");

        uint amountBnb = ((tokenPrice * amount) / 1e18);
        require(amountBnb <= address(this).balance, "exchange balance not enough");

        Allowed storage allowed = allowedExchangers[exchanger];

        if (allowed.limit > 0) {
            require(allowed.exchanged < allowed.limit, "exchange limit is over");
            uint amountExchangeLeft = allowed.limit - allowed.exchanged;
            require(amountExchangeLeft >= amountBnb, "exchanged amount invalid");
        }

        allowed.exchanged += amountBnb;
        payable(exchanger).transfer(amountBnb);
        
        emit ExchangeContractsTransferred(exchanger, tokenPrice, amount, amountBnb, allowed.limit, allowed.exchanged, block.timestamp);
    }

    function exchangeTokens(address user, uint amount, address token)
    public 
    {
        require(msg.sender == tokenAddress, "sender invalid");
        require(amount > 0, "amount invalid");
        require(user != address(0), "user invalid");

        if (token == tokenAddress) {
            require(IERC20(tokenAddress).balanceOf(user) >= amount, "tokens not enough");
            require(IERC20(tokenAddress).transferFrom(user, address(this), amount), "transfer error");
            
            uint burnsTokens = _exchange(user, amount);
            if (amount <= burnsTokens) {
                IERC20(tokenAddress).burn(amount);
            } else {
                uint amountLeft = amount - burnsTokens;
                require(IERC20(tokenAddress).transfer(user, amountLeft), "transfer left error");
                IERC20(tokenAddress).burn(burnsTokens);
            }
        } else {
            require(IERC20(tokenAddress).transferFrom(user, address(this), amount));
        }
    }

    function _exchange(address user, uint amount) private returns(uint burnsTokens) {
        require(tokenPrice > 0, "token price invalid");
        require(users[user].id > 0, "user not found");
        require(usersActivePumps[user] > 0, "active pump invalid");
        require(pumps[usersActivePumps[user]].user == user, "pump user different");

        uint amountBnb = ((tokenPrice * amount) / 1e18);
        burnsTokens = amount;

        uint amountExchangeLeft = amountBnb;
        if (users[user].referralEarningsTokens > 0 && users[user].referralEarningsTokens > users[user].referralExchangesTokens && 
            users[user].referralEarningsTokens - users[user].referralExchangesTokens > 0) {
            if (amountExchangeLeft <= users[user].referralEarningsTokens - users[user].referralExchangesTokens) {
                users[user].referralExchangesTokens += amountExchangeLeft;
                amountExchangeLeft = 0;
            } else {
                amountExchangeLeft -= users[user].referralEarningsTokens - users[user].referralExchangesTokens;
                users[user].referralExchangesTokens += users[user].referralEarningsTokens - users[user].referralExchangesTokens;
            }
        } 

        bool timer = false;
        uint amountExchange = 0;
        if (amountExchangeLeft > 0) {
            require((lastExchangedAt[user] + 1 days) < block.timestamp, "exchange period invalid");

            uint minExchangeAmount = (minExchangePercent > 0) ? (pumps[usersActivePumps[user]].amountExchangeDeposited / 100) * minExchangePercent : 0; //0%
            uint availableExchangeAmount = (pumps[usersActivePumps[user]].amountExchangeDeposited / 100) * maxExchangePercent; //30%
            if (minExchangeAmount <= amountExchangeLeft) {
                amountExchange = (availableExchangeAmount < amountExchangeLeft) ? amountExchangeLeft - availableExchangeAmount : amountExchangeLeft;
                if (exchangesAmount[user] + amountExchange > pumps[usersActivePumps[user]].amountExchangeLimit) {
                    amountExchange = pumps[usersActivePumps[user]].amountExchangeLimit - exchangesAmount[user];
                }
                lastExchangedAt[user] = block.timestamp;
                exchangesAmount[user] += amountExchange;
                amountExchangeLeft -= amountExchange;
                timer = true;
            }
        }

        if (amountExchangeLeft > 0) {
            amountBnb -= amountExchangeLeft;
            uint tokensReturn = (amountExchangeLeft * 1e18) / tokenPrice;
            if (tokensReturn > 0 && tokensReturn < burnsTokens) {
                burnsTokens -= tokensReturn;
            }
        }

        Exchange memory exchange = Exchange(nextExchangeId, user, tokenPrice, burnsTokens, amountBnb, block.timestamp);
        exchanges[nextExchangeId] = exchange;
        pumps[usersActivePumps[user]].amountExchanges += amountExchange;

        if (feeAddress[3] != address(0)) {
            require((exchange.amountSent / 1000) * feePercent3 <= address(this).balance, "fee exchange balance not enough");
            _transferBnb(feeAddress[3], (exchange.amountSent / 1000) * feePercent3);
        }
        require(exchange.amountSent <= address(this).balance, "exchange balance not enough");
        payable(exchange.user).transfer(exchange.amountSent);

        uint tempAmountExchangeLimit = (pumps[usersActivePumps[user]].amountExchangeLimit > 1e15) ? pumps[usersActivePumps[user]].amountExchangeLimit - 1e15 : pumps[usersActivePumps[user]].amountExchangeLimit; //permissible value error 0.001 BNB
        if (exchangesAmount[user] >= tempAmountExchangeLimit) {
            _withdrawRewardsPump(usersActivePumps[user], false);

            Pump storage pump = pumps[usersActivePumps[user]];
            pump.amountDeposited = pump.amountReinvested;
            pump.amountExchangeDeposited = pump.amountDeposited;
            pump.amountReinvested = 0;
            pump.amountExchangeLimit = pump.amountDeposited * 3;
            pump.reinvestCount++;
            pump.dailyPercent = getPumpPercentage(pump.amountDeposited);

            exchangesAmount[user] = 0;

            emit PumpDeposited(pump.user, pump.id, pump.dailyPercent, 0, pump.amountDeposited, pump.reinvestCount, pump.amountExchangeDeposited, pump.amountExchangeLimit, pump.amountReinvested, false);
            tokenPrice += tokenPriceAdd;
        }

        emit ExchangeTransferred(exchange.user, exchange.id, exchange.price, exchange.amountReceived, exchange.amountSent, (burnsTokens < amount) ? amount - burnsTokens : 0, pumps[usersActivePumps[exchange.user]].amountExchangeLimit, exchangesAmount[exchange.user], exchange.exchangedAt, timer);

        nextExchangeId++;
        return burnsTokens;
    }

    function reinvestTokens(address user, uint amount, address token)
    public 
    {
        require(msg.sender == tokenAddress, "sender invalid");
        require(amount > 0, "amount invalid");
        require(user != address(0), "user invalid");

        if (token == tokenAddress) {
            require(IERC20(tokenAddress).balanceOf(user) >= amount, "tokens not enough");
            require(IERC20(tokenAddress).transferFrom(user, address(this), amount), "transfer error");
            IERC20(tokenAddress).burn(amount);

            _reinvest(user, amount);
        } else {
            require(IERC20(tokenAddress).transferFrom(user, address(this), amount));
        }
    }

    function _reinvest(address user, uint amount) private {
        require(tokenPrice > 0, "token price invalid");
        require(users[user].id > 0, "user not found");
        require(usersActivePumps[user] > 0, "active pump invalid");
        require(pumps[usersActivePumps[user]].user == user, "pump user different");

        uint amountBnb = ((tokenPrice * amount) / 1e18);
        require(amountBnb >= minReinvestAmount, "min reinvest amount invalid");

        Pump storage pump = pumps[usersActivePumps[user]];
        pump.amountDeposited += amountBnb;
        pump.amountReinvested += amountBnb;
        pump.dailyPercent = getPumpPercentage(pump.amountDeposited);

        emit PumpDeposited(pump.user, pump.id, pump.dailyPercent, amountBnb, pump.amountDeposited, pump.reinvestCount, pump.amountExchangeDeposited, pump.amountExchangeLimit, pump.amountReinvested, true);

        depositsCount++;
    }

    function _withdrawRewardsPump(uint pumpId, bool reverted) private {
        (uint rewardDays, uint rewardAmount) = getRewardsAmount(pumpId);
        if (reverted) {
            require(rewardAmount > 0, "reward amount invalid");
            require(rewardDays > 0, "reward days invalid");
            require(exchangesAmount[pumps[pumpId].user] < pumps[pumpId].amountExchangeLimit, "limitation claim amount has ended, needs reinvestment");
        }
        
        if (rewardAmount > 0 && rewardDays > 0) {
            pumps[pumpId].amountWithdrawn += rewardAmount;
            pumps[pumpId].withdrawnDaysCount += rewardDays;

            IERC20(tokenAddress).mint(pumps[pumpId].user, rewardAmount);

            emit PumpRewardTransferred(pumps[pumpId].user, pumps[pumpId].id, pumps[pumpId].dailyPercent, rewardDays, rewardAmount, block.timestamp);

            _transferReferralRewards(pumps[pumpId].user, pumpId, rewardAmount, false);

            if (feeAddress[4] != address(0)) {
                IERC20(tokenAddress).mint(feeAddress[4], (rewardAmount / 10000) * feePercent4);
                if (users[feeAddress[4]].id > 0) {
                    users[feeAddress[4]].referralEarningsTokens += (((tokenPrice * rewardAmount) / 10000) * feePercent4) / 1e18;

                    emit UserReferralEarned(feeAddress[4], rewardAmount, users[feeAddress[4]].referralEarnings, users[feeAddress[4]].referralEarningsTokens, users[feeAddress[4]].referralExchangesTokens, true, true);
                }
            }
            if (feeAddress[5] != address(0)) {
                IERC20(tokenAddress).mint(feeAddress[5], (rewardAmount / 10000) * feePercent5);
                if (users[feeAddress[5]].id > 0) {
                    users[feeAddress[5]].referralEarningsTokens += (((tokenPrice * rewardAmount) / 10000) * feePercent5) / 1e18;

                    emit UserReferralEarned(feeAddress[5], rewardAmount, users[feeAddress[5]].referralEarnings, users[feeAddress[5]].referralEarningsTokens, users[feeAddress[5]].referralExchangesTokens, true, true);
                }
            }
        }
    }

    function _depositPump(uint pumpId, uint amount) private {
        Pump storage pump = pumps[pumpId];
        pump.amountDeposited += amount;
        pump.amountReinvested += amount;
        pump.dailyPercent = getPumpPercentage(pump.amountDeposited);

        emit PumpDeposited(pump.user, pump.id, pump.dailyPercent, amount, pump.amountDeposited, pump.reinvestCount, pump.amountExchangeDeposited, pump.amountExchangeLimit, pump.amountReinvested, false);

        _transferReferralRewards(pumps[pumpId].user, pumps[pumpId].id, amount, true);

        _transferBnb(feeAddress[1], (amount / 10000) * feePercent1);
        _transferBnb(feeAddress[2], (amount / 10000) * feePercent2);
        if (feeAddress[6] != address(0) && feePercent6 > 0) {
            _transferBnb(feeAddress[6], (amount / 10000) * feePercent6);
        }
        
        depositsCount++;
    }

    function _createPump(address user, uint amount) private {
        uint percentage = getPumpPercentage(amount);

        Pump memory pump = Pump(nextPumpId, user, amount, 0, 0, 0, amount, (amount * 2), 0, percentage, 0, block.timestamp);
        pumps[nextPumpId] = pump;
        usersActivePumps[user] = nextPumpId;

        emit PumpCreated(pump.user, pump.id, pump.amountDeposited, pump.dailyPercent, pump.createdAt);

        _transferReferralRewards(pump.user, nextPumpId, pump.amountDeposited, true);

        _transferBnb(feeAddress[1], (amount / 10000) * feePercent1);
        _transferBnb(feeAddress[2], (amount / 10000) * feePercent2);
        if (feeAddress[6] != address(0) && feePercent6 > 0) {
            _transferBnb(feeAddress[6], (amount / 10000) * feePercent6);
        }

        nextPumpId++;
        pumpsCount++;
        depositsCount++;
        tokenPrice += tokenPriceAdd;
    }

    function _transferReferralRewards(address user, uint pumpId, uint amount, bool deposited) private {
        address referral = users[user].referral;

        for (uint8 line = 1; line <= activeReferralLines; line++) {
            if (referral == address(0)) {
                break;
            }

            uint8 allowedLines = getReferralLines(pumps[usersActivePumps[referral]].amountDeposited);
            if (line > allowedLines) {
                referral = users[referral].referral;
                continue;
            }

            if (usersActivePumps[referral] == 0 || exchangesAmount[referral] >= pumps[usersActivePumps[referral]].amountExchangeLimit) {
                referral = users[referral].referral;
                continue;
            }

            uint rewardAmount = 0;
            if (line == 1) {
                if (deposited) {
                    rewardAmount = (amount / 10000) * referralsDeposit[1];
                } else {
                    rewardAmount = (amount / 10000) * referralsClaim[2];
                }
            } else if (line == 2) {
                if (deposited) {
                    rewardAmount = (amount / 10000) * referralsDeposit[2];
                } else {
                    rewardAmount = (amount / 10000) * referralsClaim[2];
                }
            } else if (line == 3) {
                if (deposited) {
                    rewardAmount = (amount / 10000) * referralsDeposit[3];
                } else {
                    rewardAmount = (amount / 10000) * referralsClaim[3];
                }
            } else if (line == 4) {
                if (deposited) {
                    rewardAmount = (amount / 10000) * referralsDeposit[4];
                } else {
                    rewardAmount = (amount / 10000) * referralsClaim[4];
                }
            } else if (line >= 5 && line <= 6) {
                if (deposited) {
                    rewardAmount = (amount / 10000) * referralsDeposit[5];
                } else {
                    rewardAmount = (amount / 10000) * referralsClaim[5];
                }
            } else if (line >= 7 && line <= 10) {
                if (deposited) {
                    rewardAmount = (amount / 10000) * referralsDeposit[6];
                } else {
                    rewardAmount = (amount / 10000) * referralsClaim[6];
                }
            } else if (line >= 11 && line <= 15) {
                if (deposited) {
                    rewardAmount = (amount / 10000) * referralsDeposit[7];
                } else {
                    rewardAmount = (amount / 10000) * referralsClaim[7];
                }
            } else if (line >= 16) {
                if (deposited) {
                    rewardAmount = (amount / 10000) * referralsDeposit[8];
                } else {
                    rewardAmount = (amount / 10000) * referralsClaim[8];
                }
            }
            
            if (rewardAmount > 0) {
                if (deposited) {
                    _transferBnb(referral, rewardAmount);
                    users[referral].referralEarnings += rewardAmount;

                    emit UserReferralEarned(referral, rewardAmount, users[referral].referralEarnings, users[referral].referralEarningsTokens, users[referral].referralExchangesTokens, false, false);
                } 
                else
                {
                    IERC20(tokenAddress).mint(referral, rewardAmount);
                    users[referral].referralEarningsTokens += ((tokenPrice * rewardAmount) / 1e18);

                    emit UserReferralEarned(referral, ((tokenPrice * rewardAmount) / 1e18), users[referral].referralEarnings, users[referral].referralEarningsTokens, users[referral].referralExchangesTokens, true, false);
                }
                
                emit ReferralRewardTransferred(user, referral, pumpId, line, amount, rewardAmount, deposited);
            }

            referral = users[referral].referral;
        }
    }

    function getRewardsAmount(uint pumpId) public view returns(uint rewardDays, uint rewardAmount) {
        require(pumps[pumpId].id != 0, "pump not found");

        if (tokenPrice == 0) {
            return (0, 0);
        }

        uint pumpDays = 0;
        if (pumps[pumpId].createdAt > 0) {
            pumpDays = (block.timestamp - pumps[pumpId].createdAt) / pumpPeriod;
        }

        if (pumpDays == 0) {
            return (0, 0);
        } else {
            pumpDays = pumpDays - pumps[pumpId].withdrawnDaysCount;
            if (pumpDays == 0) {
                return (0, 0);
            }

            uint amountBnb = ((pumps[pumpId].amountDeposited / 10000) * pumps[pumpId].dailyPercent) * pumpDays;
            if (amountBnb == 0) {
                return (0, 0);
            }

            rewardDays = pumpDays;
            rewardAmount = (amountBnb * 1e18) / tokenPrice;
        } 
    }

    function getReferralLines(uint amount) public view returns (uint8 lines) {
        if (amount >= 0 && amount < 15 * 1e17) {
            lines = 2;
        } else if (amount < 3 * 1e18) {
            lines = 4;
        } else if (amount < 10 * 1e18) {
            lines = 6;
        } else if (amount < maxPumpAmount) {
            lines = 8;
        } else if (amount >= maxPumpAmount) {
            lines = activeReferralLines;
        } else {
            lines = 2;
        }

        if (minReferralLines > 0 && minReferralLines > lines) {
            lines = minReferralLines;
        }
    }

    function getPumpPercentage(uint amount) public view returns(uint percentage) {
        if (amount >= 0 && amount < 15 * 1e17) {    
            percentage = percentages[1]; //0.5%
        } else if (amount < 3 * 1e18) {
            percentage = percentages[2]; //0.75%
        } else if (amount < 10 * 1e18) {
            percentage = percentages[3]; //1%
        } else if (amount < maxPumpAmount) {
            percentage = percentages[4]; //1.25%
        } else if (amount >= maxPumpAmount) {
            percentage = percentages[5]; //1.5%
        } else {
            percentage = percentages[1]; //0.5%
        }
    }
}