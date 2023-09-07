// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function burn(uint256 amount) external;
    function mint(address recipient, uint256 amount) external;
}

contract TUMain {
    mapping (address => User) public users;
    mapping (uint => address) public userIdsAddress;
    mapping (uint => Pump) public pumps;
    mapping (address => uint) public usersActivePumps;
    mapping (uint => Exchange) public exchanges;
    mapping (address => uint) public lastExchangedAt;
    mapping (address => uint) public exchangesAmount;
    mapping (uint8 => UpdateUint) public updatesUint;
    mapping (uint8 => UpdateAddress) public updatesAddress;
    mapping (uint8 => address) public feeAddress;
    mapping (uint8 => uint) public referralsDeposit;
    mapping (uint8 => uint) public referralsClaim;

    address public allowedWallet1;
    address public allowedWallet2;
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
    uint public activeReferralLines;

    event UserAdded(address indexed user, uint indexed userId, address indexed referral, uint createdAt);
    event PumpCreated(address indexed user, uint indexed pumpId, uint amount, uint dailyPercent, uint createdAt);
    event ReferralRewardTransferred(address indexed user, address indexed referral, uint indexed pumpId, uint8 line, uint amount, uint amountReward, bool deposited);
    event PumpRewardTransferred(address indexed user, uint indexed pumpId, uint dailyPercent, uint daysReward, uint amountReward, uint withdrawnAt);
    event PumpDeposited(address indexed user, uint indexed pumpId, uint dailyPercent, uint amount, uint amountDeposited, uint reinvestCount, uint amountExchangeDeposited, uint amountExchangeLimit, uint amountReinvested, bool tokens);
    event ExchangeTransferred(address indexed user, uint indexed exchangeId, uint price, uint amountReceived, uint amountSent, uint exchangeLimit, uint userExchangedAmount, uint exchangedAt, bool timer);
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
        uint amountWithdrawn; //TIGERUP
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
        uint amountReceived; //TIGERUP
        uint amountSent; //BNB
        uint exchangedAt;
    }

    struct UpdateUint {
        uint value;
        address[] confirms;
        bool success;
    }

    struct UpdateAddress {
        address value;
        address[] confirms;
        bool success;
    }

    constructor() {
        nextUserId = 1;
        nextPumpId = 1;
        nextExchangeId = 1;
        usersCount = 0;
        tokenPrice = 1e16; // 0.01 BNB
        tokenPriceAdd = 234375000000; //0.000000234375 BNB
        maxPumpAmount = 15 * 1e18; // 15 BNB
        minPumpAmount = 15 * 1e16; // 0.15 BNB
        minReinvestAmount = 1e16; // 0.01 BNB
        pumpPeriod = 1 days;
        pumpsCount = 0;
        depositsCount = 0;
        minExchangePercent = 10; // 10% - 30%
        maxExchangePercent = 30; // 30% - 100%
        activeReferralLines = 15; // 15 lines

        referralsDeposit[1] = 12; //12%
        referralsDeposit[2] = 5; //5%
        referralsDeposit[3] = 2; //2%
        referralsDeposit[4] = 5; //0.5%
        referralsDeposit[5] = 25; //0.25%
        referralsDeposit[6] = 25; //0.25%
        referralsDeposit[7] = 10; //0.1%

        referralsClaim[1] = 2; //2%
        referralsClaim[2] = 1; //1%
        referralsClaim[3] = 5; //0.5%
        referralsClaim[4] = 5; //0.5%
        referralsClaim[5] = 5; //0.5%
        referralsClaim[6] = 25; //0.25%
        referralsClaim[7] = 10; //0.1%

        allowedWallet1 = address(0x796E9A6E38A9FB58fF1F496e8fe0786D76Be1E2C);
        allowedWallet2 = address(0xf4764075146C7798e515433E729f2772b9Df252E);

        ownerAddress = msg.sender;
        tokenAddress = address(0x784460036fDD04B89255E36f728d3bFbf474b51b);

        feeAddress[1] = address(0x267d71EB3b547c1C1F60Dd6cB31d5AEaB68EEd48); // 6%
        feeAddress[2] = address(0xb5d8E0e206aF3A2759B4851d4d0120610d32FA74); // 11%
        feeAddress[3] = address(0xBe383E23143c726534c940e4F8e9FB34214C2951); // 1.25%
        feeAddress[4] = address(ownerAddress); //exchange fee
        feeAddress[5] = address(0x796E9A6E38A9FB58fF1F496e8fe0786D76Be1E2C); //claim fee
        feeAddress[6] = address(0xf4764075146C7798e515433E729f2772b9Df252E); //claim fee

        User memory user1 = User(nextUserId, address(0), 0, 0, 0, block.timestamp);
        users[ownerAddress] = user1;
        userIdsAddress[nextUserId] = ownerAddress;
        emit UserAdded(ownerAddress, user1.id, user1.referral, user1.createdAt);

        usersCount++;
        nextUserId++;

        User memory user2 = User(nextUserId, ownerAddress, 0, 0, 0, block.timestamp);
        users[allowedWallet1] = user2;
        userIdsAddress[nextUserId] = allowedWallet1;
        emit UserAdded(allowedWallet1, user2.id, user2.referral, user2.createdAt);

        usersCount++;
        nextUserId++;

        User memory user3 = User(nextUserId, allowedWallet1, 0, 0, 0, block.timestamp);
        users[address(0x88be2dE48dd696CB789DF499a3d8B63878cf9E5d)] = user3;
        userIdsAddress[nextUserId] = address(0x88be2dE48dd696CB789DF499a3d8B63878cf9E5d);
        emit UserAdded(address(0x88be2dE48dd696CB789DF499a3d8B63878cf9E5d), user3.id, user3.referral, user3.createdAt);

        usersCount++;
        nextUserId++;

        User memory user4 = User(nextUserId, address(0x88be2dE48dd696CB789DF499a3d8B63878cf9E5d), 0, 0, 0, block.timestamp);
        users[address(0x753c8d1260C87AC2a5445A86Ac4B3F89f01caAd6)] = user4;
        userIdsAddress[nextUserId] = address(0x753c8d1260C87AC2a5445A86Ac4B3F89f01caAd6);
        emit UserAdded(address(0x753c8d1260C87AC2a5445A86Ac4B3F89f01caAd6), user4.id, user4.referral, user4.createdAt);

        usersCount++;
        nextUserId++;

        User memory user5 = User(nextUserId, address(0x753c8d1260C87AC2a5445A86Ac4B3F89f01caAd6), 0, 0, 0, block.timestamp);
        users[allowedWallet2] = user5;
        userIdsAddress[nextUserId] = allowedWallet2;
        emit UserAdded(allowedWallet2, user5.id, user5.referral, user5.createdAt);

        usersCount++;
        nextUserId++;

        User memory user6 = User(nextUserId, allowedWallet2, 0, 0, 0, block.timestamp);
        users[address(0x25Bdaf6A1857F662BE6F0303fcFeFa91371e3D61)] = user6;
        userIdsAddress[nextUserId] = address(0x25Bdaf6A1857F662BE6F0303fcFeFa91371e3D61);
        emit UserAdded(address(0x25Bdaf6A1857F662BE6F0303fcFeFa91371e3D61), user6.id, user6.referral, user6.createdAt);

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

    function initializeUpdateAddress(uint8 id, address value) public onlyOwner() {
        if (id >= 1 && id <= 6) {
            address[] memory wallets;
            updatesAddress[id] = UpdateAddress(value, wallets, false);
        } else {
            revert("invalid update");
        }
    }

    function finalizeUpdateAddress(uint8 id) public {
        require(msg.sender == allowedWallet1 || msg.sender == allowedWallet2, "only allowed wallets");
        require(id > 0, "invalid id");
        require(!updatesAddress[id].success, "already finalized update Address");

        UpdateAddress storage updateAddress = updatesAddress[id];
        if (updateAddress.confirms.length == 0) {
            updateAddress.confirms.push(msg.sender);
        } else if (updateAddress.confirms.length == 1) {
            if (updateAddress.confirms[0] == msg.sender) {
                revert("confirmation already set");
            } else {
                updateAddress.confirms.push(msg.sender);
                updateAddress.success = true;

                if (id >= 1 && id <= 6) {
                    feeAddress[id] = updateAddress.value;
                } else {
                    revert("invalid update Address");
                }

                emit UpdatedAddress(id, updateAddress.value, block.timestamp);
            }
        } else {
            revert("already finalized update Address");
        }
    }

    function finalizeUpdateUint(uint8 id) public {
        require(msg.sender == allowedWallet1 || msg.sender == allowedWallet2, "only allowed wallets");
        require(id > 0, "invalid id");
        require(updatesUint[id].value > 0, "invalid initialized update uint");
        require(!updatesUint[id].success, "already finalized update uint");

        UpdateUint storage updateUint = updatesUint[id];
        if (updateUint.confirms.length == 0) {
            updateUint.confirms.push(msg.sender);
        } else if (updateUint.confirms.length == 1) {
            if (updateUint.confirms[0] == msg.sender) {
                revert("confirmation already set");
            } else {
                updateUint.confirms.push(msg.sender);
                updateUint.success = true;

                if (id == 1 && updateUint.value >= 10 && updateUint.value <= 30 && updateUint.value <= maxExchangePercent) { //10%-30%
                    minExchangePercent = updateUint.value;
                } else if (id == 2 && updateUint.value >= 30 && updateUint.value <= 100 && updateUint.value >= minExchangePercent) { //30%-100%
                    maxExchangePercent = updateUint.value;
                } else if (id == 3 && updateUint.value <= 1e14) { // X <= 0,0001 BNB
                    tokenPriceAdd = updateUint.value;
                } else if (id >= 4 && id <= 11 && updateUint.value <= 100) {
                    referralsDeposit[id - 3] = updateUint.value;
                } else if (id >= 12 && id <= 19 && updateUint.value <= 100) {
                    referralsClaim[id - 11] = updateUint.value;
                } else if (id == 20 && updateUint.value >= 10 && updateUint.value <= 50) {
                    activeReferralLines = updateUint.value; // 15 lines
                } else {
                    revert("invalid update uint");
                }

                emit UpdatedUint(id, updateUint.value, block.timestamp);
            }
        } else {
            revert("already finalized update uint");
        }
    }

    function initializeUpdateUint(uint8 id, uint value) public onlyOwner() {
        if (id >= 1 && id <= 20) {
            address[] memory wallets;
            updatesUint[id] = UpdateUint(value, wallets, false);
        } else {
            revert("invalid update");
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

    function exchangeTokens(address user, uint amount, address token)
    public 
    {
        require(msg.sender == tokenAddress, "sender invalid");
        require(amount > 0, "amount invalid");
        require(user != address(0), "user invalid");

        if (token == tokenAddress) {
            require(IERC20(tokenAddress).balanceOf(user) >= amount, "tokens not enough");
            require(IERC20(tokenAddress).transferFrom(user, address(this), amount), "transfer error");
            IERC20(tokenAddress).burn(amount);

            _exchange(user, amount);
        } else {
            require(IERC20(tokenAddress).transferFrom(user, address(this), amount));
        }
    }

    function _exchange(address user, uint amount) private {
        require(tokenPrice > 0, "token price invalid");
        require(users[user].id > 0, "user not found");
        require(usersActivePumps[user] > 0, "active pump invalid");
        require(pumps[usersActivePumps[user]].user == user, "pump user different");

        uint amountBnb = ((tokenPrice * amount) / 1e18);
        require(amountBnb <= address(this).balance, "exchange balance not enough");

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

        if (amountExchangeLeft > 0) {
            require((lastExchangedAt[user] + 1 days) < block.timestamp, "exchange period invalid");

            uint minExchangeAmount = (pumps[usersActivePumps[user]].amountExchangeDeposited / 100) * minExchangePercent; //10%
            uint availableExchangeAmount = (pumps[usersActivePumps[user]].amountExchangeDeposited / 100) * maxExchangePercent; //30%

            require(minExchangeAmount > 0, "min available exchange amount invalid");
            require(minExchangeAmount <= amountExchangeLeft, "min exchange amount invalid");
            require(availableExchangeAmount > 0, "available exchange amount invalid");
            require(availableExchangeAmount >= amountExchangeLeft, "exchange amount invalid");

            require(exchangesAmount[user] + amountExchangeLeft <= pumps[usersActivePumps[user]].amountExchangeLimit + 1e15, "limitation exchange amount has ended"); //permissible value error 0.001 BNB

            lastExchangedAt[user] = block.timestamp;
            exchangesAmount[user] += amountExchangeLeft;
        }

        Exchange memory exchange = Exchange (nextExchangeId, user, tokenPrice, amount, amountBnb, block.timestamp);
        exchanges[nextExchangeId] = exchange;
        pumps[usersActivePumps[user]].amountExchanges += amountExchangeLeft;

        if (feeAddress[4] != address(0)) {
            _transferBnb(feeAddress[4], (exchange.amountSent / 1000) * 25);
        }
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
            pump.dailyPercent = getPumpPercentage(pump.amountDeposited, pump.reinvestCount);

            exchangesAmount[user] = 0;

            emit PumpDeposited(pump.user, pump.id, pump.dailyPercent, 0, pump.amountDeposited, pump.reinvestCount, pump.amountExchangeDeposited, pump.amountExchangeLimit, pump.amountReinvested, false);
            _pumpTokenPrice();
        }

        emit ExchangeTransferred(exchange.user, exchange.id, exchange.price, exchange.amountReceived, exchange.amountSent, pumps[usersActivePumps[exchange.user]].amountExchangeLimit, exchangesAmount[exchange.user], exchange.exchangedAt, (amountExchangeLeft > 0) ? true : false);

        nextExchangeId++;
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
        pump.dailyPercent = getPumpPercentage(pump.amountDeposited, pump.reinvestCount);

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

            if (feeAddress[5] != address(0)) {
                IERC20(tokenAddress).mint(feeAddress[5], (rewardAmount / 10000) * 125);
                if (users[feeAddress[5]].id > 0) {
                    users[feeAddress[5]].referralEarningsTokens += (((tokenPrice * rewardAmount) / 10000) * 125) / 1e18;

                    emit UserReferralEarned(feeAddress[5], rewardAmount, users[feeAddress[5]].referralEarnings, users[feeAddress[5]].referralEarningsTokens, users[feeAddress[5]].referralExchangesTokens, true, true);
                }
            }
            if (feeAddress[6] != address(0)) {
                IERC20(tokenAddress).mint(feeAddress[6], (rewardAmount / 10000) * 125);
                if (users[feeAddress[6]].id > 0) {
                    users[feeAddress[6]].referralEarningsTokens += (((tokenPrice * rewardAmount) / 10000) * 125) / 1e18;

                    emit UserReferralEarned(feeAddress[6], rewardAmount, users[feeAddress[6]].referralEarnings, users[feeAddress[6]].referralEarningsTokens, users[feeAddress[6]].referralExchangesTokens, true, true);
                }
            }
            IERC20(tokenAddress).mint(pumps[pumpId].user, rewardAmount);

            emit PumpRewardTransferred(pumps[pumpId].user, pumps[pumpId].id, pumps[pumpId].dailyPercent, rewardDays, rewardAmount, block.timestamp);

            _transferReferralRewards(pumps[pumpId].user, pumpId, rewardAmount, false);
        }
    }

    function _depositPump(uint pumpId, uint amount) private {
        Pump storage pump = pumps[pumpId];
        pump.amountDeposited += amount;
        pump.amountReinvested += amount;
        pump.dailyPercent = getPumpPercentage(pump.amountDeposited, pump.reinvestCount);

        emit PumpDeposited(pump.user, pump.id, pump.dailyPercent, amount, pump.amountDeposited, pump.reinvestCount, pump.amountExchangeDeposited, pump.amountExchangeLimit, pump.amountReinvested, false);

        _transferBnb(feeAddress[1], (amount / 100) * 6);
        _transferBnb(feeAddress[2], (amount / 100) * 11);
        if (feeAddress[3] != address(0)) {
            _transferBnb(feeAddress[3], (amount / 10000) * 125); //bonus leader
        }
        
        _transferReferralRewards(pumps[pumpId].user, pumps[pumpId].id, amount, true);
        
        depositsCount++;
    }

    function _createPump(address user, uint amount) private {
        uint percentage = getPumpPercentage(amount, 0);

        Pump memory pump = Pump(nextPumpId, user, amount, 0, 0, 0, amount, (amount * 2), 0, percentage, 0, block.timestamp);
        pumps[nextPumpId] = pump;
        usersActivePumps[user] = nextPumpId;

        emit PumpCreated(pump.user, pump.id, pump.amountDeposited, pump.dailyPercent, pump.createdAt);

        _transferBnb(feeAddress[1], (amount / 100) * 6);
        _transferBnb(feeAddress[2], (amount / 100) * 11);
        if (feeAddress[3] != address(0)) {
            _transferBnb(feeAddress[3], (amount / 10000) * 125); //bonus leader
        }

        _transferReferralRewards(pump.user, nextPumpId, pump.amountDeposited, true);

        nextPumpId++;
        pumpsCount++;
        depositsCount++;
        _pumpTokenPrice();
    }

    function _transferReferralRewards(address user, uint pumpId, uint amount, bool deposited) private {
        address referral = users[user].referral;

        for (uint8 line = 1; line <= uint8(activeReferralLines); line++) {
            if (referral == address(0)) {
                break;
            }

            uint8 allowedLines = getReferralLines(pumps[usersActivePumps[referral]].amountDeposited, pumps[usersActivePumps[referral]].reinvestCount);
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
                    rewardAmount = (amount / 100) * referralsDeposit[1];
                } else {
                    rewardAmount = (amount / 100) * referralsClaim[2];
                }
            } else if (line == 2) {
                if (deposited) {
                    rewardAmount = (amount / 100) * referralsDeposit[2];
                } else {
                    rewardAmount = (amount / 100) * referralsClaim[2];
                }
            } else if (line == 3) {
                if (deposited) {
                    rewardAmount = (amount / 100) * referralsDeposit[3];
                } else {
                    rewardAmount = (amount / 1000) * referralsClaim[3];
                }
            } else if (line == 4) {
                if (deposited) {
                    rewardAmount = (amount / 1000) * referralsDeposit[4];
                } else {
                    rewardAmount = (amount / 1000) * referralsClaim[4];
                }
            } else if (line >= 5 && line <= 6) {
                if (deposited) {
                    rewardAmount = (amount / 10000) * referralsDeposit[5];
                } else {
                    rewardAmount = (amount / 1000) * referralsClaim[5];
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

    function _pumpTokenPrice() private {
        if (depositsCount <= 250000) {
            tokenPrice += 60000000000; //0.00000006 BNB
        } else if (depositsCount <= 500000) {
            tokenPrice += 150000000000; //0.00000015 BNB
        } else if (depositsCount <= 1000000) {
            tokenPrice += 187500000000; //0.0000001875 BNB
        } else if (depositsCount <= 2500000) {
            tokenPrice += 156250000000; //0.00000015625 BNB
        } else if (depositsCount <= 5000000) { 
            tokenPrice += tokenPriceAdd; //0.000000234375 BNB
        } else {
            tokenPrice += tokenPriceAdd; //0.000000234375 BNB
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

    function getReferralLines(uint amount, uint reinvestCount) public view returns (uint8 lines) {
        if (amount >= 0 && amount < _reinvestAmount(15 * 1e17, reinvestCount)) {
            lines = 2;
        } else if (amount < _reinvestAmount(3 * 1e18, reinvestCount)) {
            lines = 5;
        } else if (amount < _reinvestAmount(10 * 1e18, reinvestCount)) {
            lines = 8;
        } else if (amount < _reinvestAmount(maxPumpAmount, reinvestCount)) {
            lines = 12;
        } else if (amount >= _reinvestAmount(maxPumpAmount, reinvestCount)) {
            lines = 15;
        } else {
            lines = 2;
        }
    }

    function getPumpPercentage(uint amount, uint reinvestCount) public view returns(uint percentage) {
        if (amount >= 0 && amount < _reinvestAmount(15 * 1e17, reinvestCount)) {    
            percentage = 50; //0.5%
        } else if (amount < _reinvestAmount(3 * 1e18, reinvestCount)) {
            percentage = 75; //0.75%
        } else if (amount < _reinvestAmount(10 * 1e18, reinvestCount)) {
            percentage = 100; //1%
        } else if (amount < _reinvestAmount(maxPumpAmount, reinvestCount)) {
            percentage = 125; //1.25%
        } else if (amount >= _reinvestAmount(maxPumpAmount, reinvestCount)) {
            percentage = 150; //1.5%
        } else {
            percentage = 50; //0.5%
        }
    }

    function _reinvestAmount(uint amount, uint count) private pure returns (uint) {
        if (amount == 0) {
            return 0;
        }

        if (count == 0) {
            return amount;
        }

        uint result = amount;
        for (uint i = 1; i <= count; i++) {
            if (i == 1) {
                result = (result * 15) / 10;
            } else {
                result = (result * 125) / 100;
            }
            
        }

        return result;
    }
}