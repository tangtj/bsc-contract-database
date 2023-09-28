// SPDX-License-Identifier: MIT
// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.4;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

// File: contracts/ChickenFarm1.sol


pragma solidity ^0.8.4;


contract ChickenFarm {
    IERC20 public token;
    uint256 public eggPrice;
    uint256 public eggsCollected;

    uint256 constant private SECONDS_PER_YEAR = 31536000; // Број на секунди во година
    uint256 constant private MAX_AGE = 2 * SECONDS_PER_YEAR; // Максимална возраст на кокошката (2 години)

    mapping(address => mapping(uint256 => uint256)) public chickenAges; // Зачувајте ги возрастите на кокошките

    address private developerWallet;
    address private marketingWallet;
    address private team1Wallet;
    address private team2Wallet;
    address public owner;

    address private burningAddress = address(0x000000000000000000000000000000000000dEaD);

    modifier onlyOwner() {
    require(msg.sender == owner, "Only the owner can call this function");
    _;
}

    bool private reentrancyLock = false;

    modifier preventReentrancy() {
        require(!reentrancyLock, "Reentrant call detected");
        reentrancyLock = true;
        _;
        reentrancyLock = false;
    }

    event EggsSold(address indexed player, uint256 eggsSold, uint256 qliteAmount);

    struct UserChicken {
        uint256 eggsPerHour;
        uint256 lastEggCollectTime;
        bool isActive;
    }

    struct ChickenType {
        string name;
        uint256 price;
        uint256 eggsPerHour;
    }

    ChickenType[] public chickenTypes;

    mapping(address => UserChicken[]) public userChickens;
    mapping(address => address) public referrers;

    event ChickenBought(address indexed player, uint256 eggsPerHour, uint256 commission);
    event EggsCollected(address indexed player, uint256 eggsCollected);
    event ChickenSold(address indexed player, uint256 eggsPerHour);
    event Referral(address indexed player, address indexed referrer);

    constructor(
        address _tokenAddress,
        uint256 _initialEggPrice,
        address _developerWallet,
        address _marketingWallet,
        address _team1Wallet,
        address _team2Wallet
    ) {
        owner = msg.sender; 
        token = IERC20(_tokenAddress);
        eggPrice = _initialEggPrice;
        developerWallet = _developerWallet;
        marketingWallet = _marketingWallet;
        team1Wallet = _team1Wallet;
        team2Wallet = _team2Wallet;

        chickenTypes.push(ChickenType("Cute Chicken", 40000, 2));
        chickenTypes.push(ChickenType("X Chicken", 100000, 6));
        chickenTypes.push(ChickenType("Pirate Chicken", 250000, 18));
        chickenTypes.push(ChickenType("Hero Chicken", 700000, 54));
        chickenTypes.push(ChickenType("Knight Chicken", 2000000, 150));
        chickenTypes.push(ChickenType("King Chicken", 5000000, 350));
    }

    function buyChicken(uint256 _chickenTypeIndex) external preventReentrancy returns (uint256) {
    require(_chickenTypeIndex < chickenTypes.length, "Invalid chicken type index");
    ChickenType storage chickenType = chickenTypes[_chickenTypeIndex];
    uint256 price = chickenType.price;
    uint256 commission = 0;

    if (referrers[msg.sender] != address(0)) {
        commission = (price * 10) / 100;
    }

    // Пресметајте колку токени се плаќаат за купување на кокошката
    uint256 totalTokenPrice = price + commission;

    require(token.transferFrom(msg.sender, address(this), totalTokenPrice), "Token transfer failed");

    userChickens[msg.sender].push(UserChicken(chickenType.eggsPerHour, block.timestamp, true));

    if (commission > 0) {
        require(token.transfer(referrers[msg.sender], commission), "Token transfer to referrer failed");
    }

    emit ChickenBought(msg.sender, chickenType.eggsPerHour, commission);

    return totalTokenPrice;
}

   function collectEggs() external {
    uint256 eggsProducedTotal = 0;
    for (uint256 i = 0; i < userChickens[msg.sender].length; i++) {
        uint256 eggsProducedByChicken = calculateEggsProduced(msg.sender, i);
        eggsProducedTotal += eggsProducedByChicken;
    }
    eggsCollected += eggsProducedTotal;
    emit EggsCollected(msg.sender, eggsProducedTotal);

    // Ажурирајте возраст на кокошките
    for (uint256 i = 0; i < userChickens[msg.sender].length; i++) {
        chickenAges[msg.sender][i] += block.timestamp - userChickens[msg.sender][i].lastEggCollectTime;
        userChickens[msg.sender][i].lastEggCollectTime = block.timestamp;
    }
}

function sellChicken(uint256 _index) external preventReentrancy {
    require(_index < userChickens[msg.sender].length, "Invalid chicken index");
    UserChicken storage chicken = userChickens[msg.sender][_index];
    require(chicken.isActive, "Chicken is not active");

    uint256 timePassed = block.timestamp - chicken.lastEggCollectTime;
    uint256 chickenAge = chickenAges[msg.sender][_index] + (timePassed / 3600);

    ChickenType storage chickenType = chickenTypes[_index];
    uint256 chickenPrice = (chickenType.price * 80) / 100;
    
    // Проверете дали кокошката е продадена пред истекувањето на првата година
    if (chickenAge < SECONDS_PER_YEAR) {
        // Продажба пред истекувањето на првата година
        uint256 tokenAmount = (chickenPrice * 50) / 100;
        require(token.transfer(msg.sender, tokenAmount), "Token transfer to player failed");
        require(token.transfer(burningAddress, tokenAmount), "Token transfer to burning address failed");
    } else {
        // Продажба по истекувањето на првата година
        uint256 tokenAmount = (chickenPrice * 80) / 100;
        require(token.transfer(msg.sender, (tokenAmount * 80) / 100), "Token transfer to player failed");
        require(token.transfer(burningAddress, (tokenAmount * 20) / 100), "Token transfer to burning address failed");
    }

    chicken.isActive = false;
    emit ChickenSold(msg.sender, chickenType.eggsPerHour);
}

    function sellEggs(uint256 _eggsToSell, uint256 chickenIndex) external preventReentrancy {
    require(_eggsToSell > 0, "Invalid number of eggs to sell");
    uint256 eggsBalance = calculateEggsProduced(msg.sender, chickenIndex);
    require(_eggsToSell <= eggsBalance, "Not enough eggs to sell");

    uint256 qliteAmount = (_eggsToSell * eggPrice) / 100;
    require(token.transfer(msg.sender, qliteAmount), "Token transfer to player failed");

    eggsCollected -= _eggsToSell;

    emit EggsSold(msg.sender, _eggsToSell, qliteAmount);
}


     function setChickenPrice(uint256 _chickenTypeIndex, uint256 _newPrice) external onlyOwner {
        require(_chickenTypeIndex < chickenTypes.length, "Invalid chicken type index");
        chickenTypes[_chickenTypeIndex].price = _newPrice;
    }

    function setAllChickenPrices(uint256[] memory _newPrices) external onlyOwner {
        require(_newPrices.length == chickenTypes.length, "Invalid prices array length");
        for (uint256 i = 0; i < _newPrices.length; i++) {
            chickenTypes[i].price = _newPrices[i];
        }
    }

    function setDeveloperWallet(address _wallet) external onlyOwner {
        developerWallet = _wallet;
    }

    function setMarketingWallet(address _wallet) external onlyOwner {
        marketingWallet = _wallet;
    }

    function setTeam1Wallet(address _wallet) external onlyOwner {
        team1Wallet = _wallet;
    }

    function setTeam2Wallet(address _wallet) external onlyOwner {
        team2Wallet = _wallet;
    }

    function setReferrer(address _referrer) external {
        require(_referrer != msg.sender, "You cannot refer yourself");
        require(referrers[msg.sender] == address(0), "You already have a referrer");
        require(_referrer != address(0), "Invalid referrer address");
        referrers[msg.sender] = _referrer;

        emit Referral(msg.sender, _referrer);
    }

    function calculateEggsProduced(address player, uint256 chickenIndex) private view returns (uint256) {
    uint256 eggsProduced = 0;
    UserChicken storage chicken = userChickens[player][chickenIndex];
    if (chicken.isActive) {
        uint256 timePassed = block.timestamp - chicken.lastEggCollectTime;
        uint256 hoursPassed = timePassed / 3600;
        uint256 chickenAge = chickenAges[player][chickenIndex] + hoursPassed; // Додадете го возрастот
        if (chickenAge <= MAX_AGE) { // Кокошката има максимум 2 години
            uint256 eggsProducedByChicken = chicken.eggsPerHour * hoursPassed;
            if (chickenAge >= SECONDS_PER_YEAR) { // Проверете дали е поминала 1 година
                eggsProducedByChicken = (eggsProducedByChicken * 50) / 100; // Смали 50% од јајцата
            }
            eggsProduced += eggsProducedByChicken;
        }
    }
    return eggsProduced;
}

    function getContractBalance() external view returns (uint256) {
        return token.balanceOf(address(this));
    }

    function withdrawTokens() external onlyOwner {
        uint256 contractBalance = token.balanceOf(address(this));
        require(contractBalance > 0, "Contract has no balance");
        require(token.transfer(owner, contractBalance), "Token transfer to owner failed");
    }
}