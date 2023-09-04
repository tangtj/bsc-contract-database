// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _setOwner(msg.sender);
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) internal {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface Aggregator {
    function latestRoundData()
        external
        view
        returns (
            uint80 roundId,
            int256 answer,
            uint256 startedAt,
            uint256 updatedAt,
            uint80 answeredInRound
        );
}

abstract contract Pausable is Context {
    event Paused(address account);

    event Unpaused(address account);

    bool private _paused;

    constructor() {
        _paused = false;
    }

    modifier whenNotPaused() {
        _requireNotPaused();
        _;
    }

    modifier whenPaused() {
        _requirePaused();
        _;
    }

    function paused() public view virtual returns (bool) {
        return _paused;
    }

    function _requireNotPaused() internal view virtual {
        require(!paused(), "Pausable: paused");
    }

    function _requirePaused() internal view virtual {
        require(paused(), "Pausable: not paused");
    }

    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}

contract CXCPresale is Ownable, Pausable {
    IERC20 public token;
    IERC20 public usdt;

    uint256 public maxTokensToBuy;
    uint256 public totalTokensSold;
    uint256 public startTime;
    uint256 public endTime;

    uint256 public currentStep;
    uint256 public referralPercentage;
    uint256 public totalReferralAmount;
    uint256 public withdrawReferralAmount;

    Aggregator public aggregatorInterface;

    event MaxTokensUpdated(
        uint256 prevValue,
        uint256 newValue,
        uint256 timestamp
    );
    event WithdrawReferralRewards(
        address indexed user,
        uint256 amount,
        uint256 timestamp
    );

    constructor() {
        token = IERC20(0x4e89091Aa723fbcBF48236F9AD93dc6a00fF9605);
        usdt = IERC20(0x55d398326f99059fF775485246999027B3197955);
        aggregatorInterface = Aggregator(
            0x0567F2323251f0Aab15c8dFb1967E4e8A7D42aeE
        );
        maxTokensToBuy = 25000 ether;
        startTime = 1693728000;
        endTime = 1696132740;
        referralPercentage = 1000;
        setRounds();
    }

    struct round {
        uint256 amount;
        uint256 rate;
        uint256 soldToken;
    }

    struct user {
        uint256 amount;
        uint256 remaining;
        uint256 buyTime;
        uint256 checkTime;
        uint256 quarter;
    }
    mapping(address => user[]) public deposits;
    mapping(address => uint256) private _referralRewards;
    mapping(address => bool) private checkReferral;

    round[] public rounds;

    function setRounds() private {
        rounds.push(round({amount: 25000000 ether, rate: 3030, soldToken: 0}));
        rounds.push(round({amount: 40000000 ether, rate: 2500, soldToken: 0}));
        rounds.push(round({amount: 10000000 ether, rate: 1739, soldToken: 0}));
    }

    function setcurrentStep(uint256 step) external onlyOwner {
        round storage setRound = rounds[step];
        require(step < rounds.length, "Invalid rounds Id");
        require(step != currentStep, "This round is already active");
        uint256 ourAllowance = token.allowance(_msgSender(), address(this));
        require(
            setRound.amount <= ourAllowance,
            "Make sure to add enough allowance"
        );
        token.transferFrom(_msgSender(), address(this), setRound.amount);
        currentStep = step;
    }

    function updateRounds(
        uint256 step,
        uint256 amount,
        uint256 rate
    ) external onlyOwner {
        require(step < rounds.length, "Invalid rounds Id");
        require(amount > 0 || rate > 0, "Invalid value");
        round storage setRound = rounds[step];
        if (amount > 0) setRound.amount = amount;
        if (rate > 0) setRound.rate = rate;
    }

    function changeMaxTokensToBuy(uint256 _maxTokensToBuy) external onlyOwner {
        require(_maxTokensToBuy > 0, "Zero max tokens to buy value");
        maxTokensToBuy = _maxTokensToBuy;
        emit MaxTokensUpdated(maxTokensToBuy, _maxTokensToBuy, block.timestamp);
    }

    function changeSaleStartTime(uint256 _startTime) external onlyOwner {
        require(block.timestamp <= _startTime, "Sale time in past");
        startTime = _startTime;
    }

    function changeSaleEndTime(uint256 _endTime) external onlyOwner {
        require(block.timestamp <= _endTime, "end time in past");
        require(_endTime > startTime, "Invalid endTime");
        endTime = _endTime;
    }

    function updateReferralPercentage(uint256 _newPercentage)
        external
        onlyOwner
    {
        referralPercentage = _newPercentage;
    }

    function pause() external onlyOwner returns (bool success) {
        _pause();
        return true;
    }

    function unpause() external onlyOwner returns (bool success) {
        _unpause();
        return true;
    }

    function withdrawBNB() public onlyOwner {
        require(address(this).balance > 0, "contract balance is 0");
        payable(owner()).transfer(address(this).balance);
    }

    function withdrawTokens(address _token, uint256 amount) external onlyOwner {
        require(isContract(_token), "Invalid contract address");
        require(
            IERC20(_token).balanceOf(address(this)) >= amount,
            "Insufficient tokens"
        );
        IERC20(_token).transfer(_msgSender(), amount);
    }

    function isContract(address _addr) private view returns (bool iscontract) {
        uint32 size;
        assembly {
            size := extcodesize(_addr)
        }
        return (size > 0);
    }

    modifier checkSaleState(uint256 amount) {
        require(
            block.timestamp >= startTime && block.timestamp <= endTime,
            "Invalid time for buying"
        );
        require(amount > 0, "Invalid amount");
        _;
    }

    modifier checkClaimTime(uint256 id) {
        user memory users = deposits[_msgSender()][id];
        require(users.remaining > 0, "You have no remaining tokens");
        require(
            block.timestamp > users.checkTime + 91.31 days,
            "claim time not reached yet"
        );
        _;
    }

    function removeId(uint256 indexnum) internal {
        for (uint256 i = indexnum; i < deposits[_msgSender()].length - 1; i++) {
            deposits[_msgSender()][i] = deposits[_msgSender()][i + 1];
        }
        deposits[_msgSender()].pop();
    }

    function buyWithUSDT(uint256 amount, address _referral)
        external
        checkSaleState(amount)
        whenNotPaused
    {
        uint256 numOfTokens = calculateToken(amount);
        require(numOfTokens <= maxTokensToBuy, "max tokens buy");
        uint256 ourAllowance = usdt.allowance(_msgSender(), address(this));
        require(amount <= ourAllowance, "Make sure to add enough allowance");
        uint256 instantTokens = (numOfTokens * 20) / 100;
        uint256 remaingTokens = numOfTokens - instantTokens;
        usdt.transferFrom(_msgSender(), address(this), amount);
        token.transfer(_msgSender(), instantTokens);
        deposits[_msgSender()].push(
            user(
                numOfTokens,
                remaingTokens,
                block.timestamp,
                block.timestamp,
                0
            )
        );
        referralAmount(numOfTokens, _referral);
        rounds[currentStep].soldToken += numOfTokens;
        totalTokensSold += numOfTokens;
    }

    function buyWithBNB(address _referral)
        external
        payable
        checkSaleState(msg.value)
        whenNotPaused
    {
        uint256 bnbToUsdt = (getLatestPrice() * msg.value) / 1e8;
        uint256 numOfTokens = calculateToken(bnbToUsdt);
        require(numOfTokens <= maxTokensToBuy, "max tokens buy");
        uint256 instantTokens = (numOfTokens * 20) / 100;
        token.transfer(_msgSender(), instantTokens);
        uint256 remaingTokens = numOfTokens - instantTokens;
        deposits[_msgSender()].push(
            user(
                numOfTokens,
                remaingTokens,
                block.timestamp,
                block.timestamp,
                0
            )
        );
        referralAmount(numOfTokens, _referral);
        rounds[currentStep].soldToken += numOfTokens;
        totalTokensSold += numOfTokens;
    }

    function claim(uint256 id) external checkClaimTime(id) {
        user storage users = deposits[_msgSender()][id];
        uint256 quartersTokens = (users.amount * 10) / 100;
        token.transfer(_msgSender(), quartersTokens);
        users.remaining = users.remaining - quartersTokens;
        users.checkTime = block.timestamp;
        users.quarter += 1;
        if (users.quarter == 8) {
            removeId(id);
        }
    }

    function referralAmount(uint256 amount, address _referral) private {
        if (
            !checkReferral[_msgSender()] &&
            _referral != address(0) &&
            _referral != _msgSender()
        ) {
            uint256 refferTax = (amount * referralPercentage) / 10000;
            _referralRewards[_referral] += refferTax;
            totalReferralAmount += refferTax;
        }
        checkReferral[_msgSender()] = true;
    }

    function withdrawReferral() external whenNotPaused {
        uint256 rewards = _referralRewards[_msgSender()];
        require(rewards > 0, "You do not have referral rewards.");
        token.transfer(_msgSender(), rewards);
        withdrawReferralAmount += rewards;
        emit WithdrawReferralRewards(_msgSender(), rewards, block.timestamp);
        _referralRewards[_msgSender()] = 0;
    }

    function getContractBalacne() public view returns (uint256 cxc) {
        return token.balanceOf(address(this));
    }

    function calculateToken(uint256 _usdtAmount) public view returns (uint256) {
        uint256 numOfTokens = _usdtAmount * rounds[currentStep].rate;
        return (numOfTokens / 100);
    }

    function bnbBuyHelper(uint256 amount)
        external
        view
        returns (uint256 numOfTokens)
    {
        uint256 bnbToUsdt = (getLatestPrice() * amount) / 1e8;
        numOfTokens = calculateToken(bnbToUsdt);
    }

    function getLatestPrice() public view returns (uint256) {
        (, int256 price, , , ) = aggregatorInterface.latestRoundData();
        return uint256(price);
    }

    function getCxcBalance() public view returns (uint256 cxcBalance) {
        cxcBalance = token.balanceOf(address(this));
    }

    function getUsdtBalance() public view returns (uint256 cxcBalance) {
        cxcBalance = usdt.balanceOf(address(this));
    }

    function getBnbBalance() public view returns (uint256 BNB) {
        BNB = address(this).balance;
    }

    function totalRounds() public view returns (uint256 _rounds) {
        _rounds = rounds.length;
    }

    function referralOf(address account) external view returns (uint256) {
        return _referralRewards[account];
    }

    function userDepositIndex(address _user) public view returns (uint256) {
        return deposits[_user].length;
    }

    receive() external payable {}
}