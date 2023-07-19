// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

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

    constructor(address newOwner) {
        _setOwner(newOwner);
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

interface AggregatorV3Interface {
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

contract BlueBerryWallet is Ownable, Pausable {
    IERC20 public busd;
    IERC20 public usdt;
    IERC20 public usdc;

    uint256 public totalTokensSold;

    uint256 public startTime;

    uint256 public endTime;

    address public paymentWallet;

    uint256 public referralPercentage;

    address public developerWallet;

    event SaleTimeSet(uint256 _start, uint256 _end, uint256 timestamp);
    event NewDeposit(address indexed user, uint256 amount, uint256 timestamp);
    event WithdrawReferralRewards(
        address indexed user,
        uint256 amount,
        uint256 timestamp
    );
    event SaleTimeUpdated(
        bytes32 indexed key,
        uint256 prevValue,
        uint256 newValue,
        uint256 timestamp
    );
    event TokensBought(
        address indexed user,
        uint256 indexed amountPaid,
        uint256 indexed purchaseToken,
        uint256 timestamp
    );
    event WithdrawCapitals(
        address indexed user,
        uint256 amount,
        uint256 timestamp
    );
    event TokensClaimed(
        address indexed user,
        uint256 amount,
        uint256 timestamp
    );
    event SetPlan(
        uint256 indexed apy,
        uint256 indexed time,
        uint256 minimumDeposit,
        uint256 maximumDeposit,
        bool paused
    );

    enum TokenType {
        BUSD,
        USDT,
        BNB,
        USDC
    }

    AggregatorV3Interface priceFeedBNBUSD;
    AggregatorV3Interface priceFeedBUSDUSD;
    AggregatorV3Interface priceFeedUSDTUSD;
    AggregatorV3Interface priceFeedUSDCUSD;

    constructor() Ownable(_msgSender()) {
        initialize();
    }

    struct plans {
        uint256 apy;
        uint256 time;
        uint256 minDeposit;
        uint256 maxDeposit;
        uint256 addPlanTimestamp;
        bool paused;
    }

    struct user {
        uint256 amount;
        uint256 timestamp;
        uint256 step;
        uint256 depositTime;
        TokenType tokenType;
    }
    mapping(address => user[]) public investment;
    plans[] public plansData;

    mapping(address => uint256) private _referralRewards;
    mapping(address => bool) public checkReferral;

    function removeId(uint256 indexnum) internal {
        for (
            uint256 i = indexnum;
            i < investment[_msgSender()].length - 1;
            i++
        ) {
            investment[_msgSender()][i] = investment[_msgSender()][i + 1];
        }
        investment[_msgSender()].pop();
    }

    function initialize() private {
        priceFeedBNBUSD = AggregatorV3Interface(
            0x0567F2323251f0Aab15c8dFb1967E4e8A7D42aeE
        );
        priceFeedBUSDUSD = AggregatorV3Interface(
            0xcBb98864Ef56E9042e7d2efef76141f15731B82f
        );
        priceFeedUSDTUSD = AggregatorV3Interface(
            0xB97Ad0E74fa7d920791E90258A6E2085088b4320
        );
        priceFeedUSDCUSD = AggregatorV3Interface(
            0x51597f405303C4377E36123cBc172b13269EA163
        );

        busd = IERC20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
        usdt = IERC20(0x55d398326f99059fF775485246999027B3197955);
        usdc = IERC20(0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d);
        paymentWallet = owner();
        developerWallet = owner();
        referralPercentage = 5;
        startTime = block.timestamp;
        endTime = block.timestamp + 730 days;
        plansData.push(
            plans({
                apy: 500,
                time: 2629743,
                minDeposit: 30000000000000000000,
                maxDeposit: 5000000000000000000000,
                paused: false,
                addPlanTimestamp: block.timestamp
            })
        );
        plansData.push(
            plans({
                apy: 800,
                time: 5259486,
                minDeposit: 40000000000000000000,
                maxDeposit: 10000000000000000000000,
                paused: false,
                addPlanTimestamp: block.timestamp
            })
        );
        plansData.push(
            plans({
                apy: 1000,
                time: 7889229,
                minDeposit: 50000000000000000000,
                maxDeposit: 0,
                paused: false,
                addPlanTimestamp: block.timestamp
            })
        );
        plansData.push(
            plans({
                apy: 1000,
                time: 15778458,
                minDeposit: 10000000000000000000000,
                maxDeposit: 0,
                paused: false,
                addPlanTimestamp: block.timestamp
            })
        );
        emit SaleTimeSet(startTime, endTime, block.timestamp);
    }

    function changePaymentWallet(address _newPaymentWallet) external onlyOwner {
        require(_newPaymentWallet != address(0), "address cannot be zero");
        require(
            _newPaymentWallet != paymentWallet,
            "This address is already fixed"
        );
        paymentWallet = _newPaymentWallet;
    }

    function changeDevWallet(address _newAdmin) external onlyOwner {
        require(_newAdmin != address(0), "address cannot be zero");
        require(_newAdmin != developerWallet, "This address is already fixed");
        developerWallet = _newAdmin;
    }

    function setPlans(
        uint256 _apy,
        uint256 _time,
        uint256 _minDeposit,
        uint256 _maxDeposit,
        bool _paused
    ) external onlyAdmin {
        require(_apy != 0 && _time != 0, "Please set a valid APY and time");
        plansData.push(
            plans({
                apy: _apy,
                time: _time,
                minDeposit: _minDeposit,
                maxDeposit: _maxDeposit,
                paused: _paused,
                addPlanTimestamp: block.timestamp
            })
        );
        emit SetPlan(_apy, _time, _minDeposit, _maxDeposit, _paused);
    }

    function updateReferralPercentage(uint256 _newPercentage)
        external
        onlyOwner
    {
        referralPercentage = _newPercentage;
    }

    modifier onlyAdmin() {
        require(
            owner() == _msgSender() || developerWallet == _msgSender(),
            "Ownable: caller is not the owner or developer"
        );
        _;
    }

    modifier checkSaleState(uint256 value, uint256 amount) {
        require(
            block.timestamp >= startTime && block.timestamp <= endTime,
            "Invalid time for buying"
        );
        require(value > 0 || amount > 0, "Invalid sale amount");
        _;
    }

    modifier checkPlans(uint256 step, uint256 amount) {
        require(!plansData[step].paused, "This plan is currently inactive");
        if (msg.value == 0) {
            if (plansData[step].maxDeposit != 0) {
                require(plansData[step].maxDeposit >= amount, "Max Stake");
                require(plansData[step].minDeposit <= amount, "Min Stake");
            } else {
                require(
                    plansData[step].minDeposit <= amount,
                    "Minimum Stake in premium plan"
                );
            }
        } else {
            amount = (msg.value * getLatestPriceBNB()) / 1e8;
            require(plansData[step].maxDeposit >= amount, "Max Stake BNB");
            require(plansData[step].minDeposit <= amount, "Min Stake BNB");
        }
        _;
    }

    function pausedPlan(uint256 _step) external onlyAdmin {
        require(!plansData[_step].paused, "Plan is already inactive");
        plansData[_step].paused = true;
    }

    function unpausedPlan(uint256 _step) external onlyAdmin {
        require(plansData[_step].paused, "Plan is already active");
        plansData[_step].paused = false;
    }

    function updatePlan(
        uint256 step,
        uint256 _apy,
        uint256 _time,
        uint256 _minDeposit,
        uint256 _maxDeposit
    ) external onlyAdmin {
        require(step < plansData.length, "Invalid step");
        if (_apy > 0) plansData[step].apy = _apy;
        if (_time > 0) plansData[step].time = _time;
        if (_minDeposit > 0) plansData[step].minDeposit = _minDeposit;
        if (_maxDeposit > 0) plansData[step].maxDeposit = _maxDeposit;
        plansData[step].paused = false;
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(getContractBNBBalance() >= amount, "Low balance");
        (bool success, ) = recipient.call{value: amount}("");
        require(success, "BNB Payment failed");
    }

    function pause() external onlyAdmin returns (bool success) {
        _pause();
        return true;
    }

    function unpause() external onlyAdmin returns (bool success) {
        _unpause();
        return true;
    }

    function changeSaleTimes(uint256 _startTime, uint256 _endTime)
        external
        onlyAdmin
    {
        require(_startTime > 0 || _endTime > 0, "Invalid parameters");
        if (_startTime > 0) {
            require(block.timestamp < startTime, "Sale already started");
            require(block.timestamp < _startTime, "Sale time in past");
            uint256 prevValue = startTime;
            startTime = _startTime;
            emit SaleTimeUpdated(
                bytes32("START"),
                prevValue,
                _startTime,
                block.timestamp
            );
        }

        if (_endTime > 0) {
            require(block.timestamp < endTime, "Sale already ended");
            require(_endTime > startTime, "Invalid endTime");
            uint256 prevValue = endTime;
            endTime = _endTime;
            emit SaleTimeUpdated(
                bytes32("END"),
                prevValue,
                _endTime,
                block.timestamp
            );
        }
    }

    function getContractBalancebusd() public view returns (uint256) {
        return busd.balanceOf(address(this));
    }

    function getContractBalanceUsdt() public view returns (uint256) {
        return usdt.balanceOf(address(this));
    }

    function getContractBalanceusdc() public view returns (uint256) {
        return usdc.balanceOf(address(this));
    }

    function withdrawbusd() public onlyOwner {
        require(getContractBalancebusd() > 0, "contract balance is 0");
        busd.transfer(_msgSender(), getContractBalancebusd());
    }

    function withdrawTokens(address _token, uint256 _amount)
        external
        onlyOwner
    {
        require(_token != address(0), "token address is null");
        IERC20 newToken = IERC20(_token);
        require(
            _amount > 0 && newToken.balanceOf(address(this)) >= _amount,
            "Amount should not be zero or more than contract"
        );
        newToken.transfer(_msgSender(), _amount);
    }

    function getContractBNBBalance() public view returns (uint256 BNB) {
        return address(this).balance;
    }

    function withdrawBNB() public onlyOwner {
        require(getContractBNBBalance() > 0, "contract balance is ZERO");
        payable(owner()).transfer(getContractBNBBalance());
    }

    function getLatestPriceBNB() public view returns (uint256) {
        (, int256 price, , , ) = priceFeedBNBUSD.latestRoundData();
        return uint256(price);
    }

    function getLatestPriceBUSDUSD() public view returns (uint256) {
        (, int256 price, , , ) = priceFeedBUSDUSD.latestRoundData();
        return uint256(price);
    }

    function getLatestPriceUSDTUSD() public view returns (uint256) {
        (, int256 price, , , ) = priceFeedUSDTUSD.latestRoundData();
        return uint256(price);
    }

    function getLatestPriceUSDCUSD() public view returns (uint256) {
        (, int256 price, , , ) = priceFeedUSDCUSD.latestRoundData();
        return uint256(price);
    }

    function getTokenPriceUSD(address _token)
        public
        view
        returns (uint256 usdPrice)
    {
        IERC20 newToken = IERC20(_token);
        require(
            newToken == busd || newToken == usdt || newToken == usdc,
            "unsupported token address"
        );
        if (busd == newToken) {
            usdPrice = getLatestPriceBUSDUSD();
        } else if (usdc == newToken) {
            usdPrice = getLatestPriceUSDCUSD();
        } else {
            usdPrice = getLatestPriceUSDTUSD();
        }
        return usdPrice;
    }

    function buyWithToken(
        uint256 amount,
        address _src,
        address _get
    ) external checkSaleState(0, amount) whenNotPaused {
        IERC20 fromToken = IERC20(_src);
        IERC20 getToken = IERC20(_get);
        require(
            fromToken == busd || fromToken == usdt || fromToken == usdc,
            "from token is unsupported"
        );
        require(
            getToken == busd || getToken == usdt || getToken == usdc,
            "get token is unsupported"
        );
        uint256 fromUsdPrice = getTokenPriceUSD(_src);
        uint256 toUsdPrice = getTokenPriceUSD(_get);
        uint256 numberOfTokens = (fromUsdPrice * amount) / toUsdPrice;
        require(numberOfTokens > 0, "The amount should be grater");
        require(
            numberOfTokens <= getToken.balanceOf(address(this)),
            "insufficient amount is in the contract."
        );
        uint256 ourAllowance = fromToken.allowance(_msgSender(), address(this));
        require(amount <= ourAllowance, "Make sure to add enough allowance");
        fromToken.transferFrom(_msgSender(), paymentWallet, amount);
        getToken.transfer(_msgSender(), numberOfTokens);
        totalTokensSold += numberOfTokens;
        emit TokensBought(
            _msgSender(),
            amount,
            numberOfTokens,
            block.timestamp
        );
    }

    function buyWithBNB(address _token, uint256 amount)
        external
        payable
        checkSaleState(msg.value, amount)
        whenNotPaused
    {
        require(_token != address(0), "null address");
        IERC20 newToken = IERC20(_token);
        require(
            newToken == busd || newToken == usdt || newToken == usdc,
            "unsupported token address"
        );
        uint256 tokenUsdPrice = getTokenPriceUSD(_token);
        uint256 bnbUsdPrice = getLatestPriceBNB();
        uint256 numberOfTokens;
        uint256 enteredAmount;
        if (msg.value > 0) {
            enteredAmount = msg.value;
            numberOfTokens = (bnbUsdPrice / tokenUsdPrice) * enteredAmount;
            require(numberOfTokens > 0, "The amount should be grater");
            require(
                numberOfTokens <= newToken.balanceOf(address(this)),
                "insufficient amount is in the contract."
            );
            sendValue(payable(paymentWallet), enteredAmount);
            newToken.transfer(_msgSender(), numberOfTokens);
            totalTokensSold += numberOfTokens;
        } else {
            enteredAmount = amount;
            uint256 ourAllowance = newToken.allowance(
                _msgSender(),
                address(this)
            );
            require(
                enteredAmount <= ourAllowance,
                "Make sure to add enough allowance"
            );
            numberOfTokens = (tokenUsdPrice * enteredAmount) / bnbUsdPrice;
            require(numberOfTokens > 0, "The token amount should be grater");
            newToken.transferFrom(_msgSender(), paymentWallet, enteredAmount);
            sendValue(payable(_msgSender()), numberOfTokens);
        }
        emit TokensBought(
            _msgSender(),
            enteredAmount,
            numberOfTokens,
            block.timestamp
        );
    }

    function invest(
        TokenType tokenType,
        uint256 _amount,
        uint8 step,
        address _referral
    ) external payable checkPlans(step, _amount) whenNotPaused {
        require(
            TokenType.BUSD == tokenType ||
                TokenType.USDT == tokenType ||
                TokenType.BNB == tokenType ||
                TokenType.USDC == tokenType,
            "Token type unsupported"
        );
        require(step < plansData.length, "Invalid step");

        if (TokenType.BUSD == tokenType) {
            require(_amount > 0, "please stake valid amount");
            uint256 ourAllowance = busd.allowance(_msgSender(), address(this));
            require(
                _amount <= ourAllowance,
                "Make sure to add enough allowance of busd"
            );
            busd.transferFrom(_msgSender(), address(this), _amount);
        } else if (TokenType.USDT == tokenType) {
            require(_amount > 0, "please stake valid amount");
            uint256 ourAllowance = usdt.allowance(_msgSender(), address(this));
            require(
                _amount <= ourAllowance,
                "Make sure to add enough allowance of usdt"
            );
            usdt.transferFrom(_msgSender(), address(this), _amount);
        } else if (TokenType.USDC == tokenType) {
            require(_amount > 0, "please stake valid amount");
            uint256 ourAllowance = usdc.allowance(_msgSender(), address(this));
            require(
                _amount <= ourAllowance,
                "Make sure to add enough allowance of usdc"
            );
            usdc.transferFrom(_msgSender(), address(this), _amount);
        } else {
            require(msg.value > 0, "Please provide a bnb value");
            require(
                step == 0 || step == 1,
                "Invalid plan for selected currency"
            );
            payable(address(this)).transfer(msg.value);
        }
        uint256 investAmount = TokenType.BNB == tokenType ? msg.value : _amount;
        investment[_msgSender()].push(
            user({
                amount: investAmount,
                timestamp: block.timestamp,
                step: step,
                depositTime: block.timestamp,
                tokenType: tokenType
            })
        );
        if (
            !checkReferral[_msgSender()] &&
            _referral != address(0) &&
            _referral != _msgSender()
        ) {
            uint256 refferTax = (investAmount * referralPercentage) / 100;
            _referralRewards[_referral] += refferTax;
        }
        checkReferral[_msgSender()] = true;
        emit NewDeposit(_msgSender(), investAmount, block.timestamp);
    }

    modifier checkWithdrawTime(uint256 id) {
        require(id < investment[_msgSender()].length, "Invalid enter Id");
        user memory users = investment[_msgSender()][id];
        require(
            users.depositTime + plansData[users.step].time < block.timestamp,
            "Withdrawal time has not yet expired."
        );
        _;
    }

    function withdrawCapital(uint256 id)
        external
        checkWithdrawTime(id)
        whenNotPaused
        returns (bool success)
    {
        user memory users = investment[_msgSender()][id];
        require(id < investment[_msgSender()].length, "Invalid enter Id");
        uint256 withdrawalAmount;
        if (TokenType.BUSD == users.tokenType) {
            (uint256 rewards1, , , ) = calculateReward(_msgSender(), id);
            withdrawalAmount = users.amount + rewards1;
            require(
                withdrawalAmount <= getContractBalancebusd(),
                "Insufficient busd amount is in the contract"
            );
            busd.transfer(_msgSender(), withdrawalAmount);
        } else if (TokenType.USDT == users.tokenType) {
            (, uint256 rewards2, , ) = calculateReward(_msgSender(), id);
            withdrawalAmount = users.amount + rewards2;
            require(
                withdrawalAmount <= getContractBalanceUsdt(),
                "Insufficient usdt amount is in the contract"
            );
            usdt.transfer(_msgSender(), withdrawalAmount);
        } else if (TokenType.USDC == users.tokenType) {
            (, , , uint256 rewards4) = calculateReward(_msgSender(), id);
            withdrawalAmount = users.amount + rewards4;
            require(
                withdrawalAmount <= getContractBalanceusdc(),
                "Insufficient usdc amount is in the contract"
            );
            usdc.transfer(_msgSender(), withdrawalAmount);
        } else {
            (, , uint256 rewards3, ) = calculateReward(_msgSender(), id);
            withdrawalAmount = users.amount + rewards3;
            require(
                withdrawalAmount <= getContractBNBBalance(),
                "Insufficient bnb amount is in the contract"
            );
            payable(msg.sender).transfer(withdrawalAmount);
        }
        emit WithdrawCapitals(_msgSender(), withdrawalAmount, block.timestamp);
        removeId(id);
        return true;
    }

    function withdrawRewards() external whenNotPaused returns (bool success) {
        uint256 index = investment[_msgSender()].length;
        require(index > 0, "You have not deposited fund");
        uint256 rewards1;
        uint256 rewards2;
        uint256 rewards3;
        uint256 rewards4;
        for (uint256 i = 0; i < index; i++) {
            user storage users = investment[_msgSender()][i];
            (uint256 busdRewards, , , ) = calculateReward(_msgSender(), i);
            (, uint256 usdtRewards, , ) = calculateReward(_msgSender(), i);
            (, , uint256 bnbRewards, ) = calculateReward(_msgSender(), i);
            (, , , uint256 usdcRewards) = calculateReward(_msgSender(), i);
            rewards1 += busdRewards;
            rewards2 += usdtRewards;
            rewards3 += bnbRewards;
            rewards4 += usdcRewards;
            users.timestamp = block.timestamp;
        }
        require(
            rewards1 > 0 || rewards2 > 0 || rewards3 > 0 || rewards4 > 0,
            "you have no rewards"
        );
        if (rewards1 > 0) busd.transfer(_msgSender(), rewards1);
        if (rewards2 > 0) usdt.transfer(_msgSender(), rewards2);
        if (rewards4 > 0) usdc.transfer(_msgSender(), rewards4);
        if (rewards3 > 0) sendValue(payable(_msgSender()), rewards3);

        emit TokensClaimed(
            _msgSender(),
            (rewards1 + rewards2 + rewards3 + rewards4),
            block.timestamp
        );
        return true;
    }

    function calculateReward(address _user, uint256 id)
        public
        view
        returns (
            uint256 busdRewards,
            uint256 usdtRewards,
            uint256 bnbRewards,
            uint256 usdcRewards
        )
    {
        require(id < investment[_user].length, "Invalid Id");
        user memory users = investment[_user][id];
        uint256 time = block.timestamp - users.timestamp;
        uint256 DIVIDER = 10000;
        if (users.tokenType == TokenType.BUSD) {
            busdRewards =
                (users.amount * plansData[users.step].apy * time) /
                DIVIDER /
                30.44 days;
        } else if (users.tokenType == TokenType.USDT) {
            usdtRewards =
                (users.amount * plansData[users.step].apy * time) /
                DIVIDER /
                30.44 days;
        } else if (users.tokenType == TokenType.USDC) {
            usdcRewards =
                (users.amount * plansData[users.step].apy * time) /
                DIVIDER /
                30.44 days;
        } else {
            bnbRewards =
                (users.amount * plansData[users.step].apy * time) /
                DIVIDER /
                30.44 days;
        }
        return (busdRewards, usdtRewards, bnbRewards, usdcRewards);
    }

    function withdrawReferral() external whenNotPaused {
        uint256 rewards = _referralRewards[_msgSender()];
        require(rewards > 0, "You do not have referral rewards.");
        busd.transfer(_msgSender(), rewards);
        emit WithdrawReferralRewards(_msgSender(), rewards, block.timestamp);
        _referralRewards[_msgSender()] = 0;
    }

    function userIndex(address _user) public view returns (uint256) {
        return investment[_user].length;
    }

    function depositAddAmount(address _user)
        public
        view
        returns (uint256 amount)
    {
        uint256 index = investment[_user].length;
        for (uint256 i = 0; i < index; i++) {
            user memory users = investment[_user][i];
            amount += users.amount;
        }
        return amount;
    }

    function referralOf(address account) external view returns (uint256) {
        return _referralRewards[account];
    }

    function numberOfPlans() public view returns (uint256) {
        return plansData.length;
    }

    receive() external payable {}
}