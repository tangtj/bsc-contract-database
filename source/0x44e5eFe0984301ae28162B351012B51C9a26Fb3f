// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function balanceOf(address account) external view returns (uint256);
}

contract DarkPresaleSeed {
    address payable public owner;
    address public darkTokenAddress;
    address public usdtTokenAddress;
    bool private _locked;

    uint256 public constant TOTAL_DARK_SUPPLY = 10000 * 10**18;
    uint256 public constant TGE_AMOUNT = (TOTAL_DARK_SUPPLY * 10) / 100;
    uint256 public constant VESTING_AMOUNT_PER_MONTH =
        (TOTAL_DARK_SUPPLY * 20) / 100 / 4;
    uint256 public constant VESTING_INTERVAL = 30 days;
    uint256 public constant SEED_ROUND_DURATION = 21 days;
    uint256 public constant MIN_DARK_AMOUNT = 10 * 10**18;
    uint256 public constant DARK_TO_USDT_RATE = 357; // 1 USDT = 0.357 DARK
    uint256 public constant DARK_TO_BNB_RATE = 85;
    uint256 public constant BNB_TO_USDT_RATE = 240;
    uint256 public startTime;
    uint256 public endTime;
    uint256 public vestingStartTime;
    uint256 public totalSoldDark;
    bool public tradingEnabled;
    mapping(address => uint256) public darkBalances;

    event TokensPurchased(address indexed buyer, uint256 darkAmount);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can perform this action");
        _;
    }

    constructor() {
        owner = payable(msg.sender);
        darkTokenAddress = 0x3096e7fFa852bf9e5cc1758974a0e905ee5d4c18; 
        usdtTokenAddress = 0x55d398326f99059fF775485246999027B3197955;
        startTime = 1693072800; //to be changed
        endTime = 1694887200; // to be changed1694887200
    }

    function calculateDarkAmount(uint256 usdtAmount)
        public
        pure
        returns (uint256)
    {
        return usdtAmount * DARK_TO_USDT_RATE;
    }

    function calculateUsdtAmount(uint256 darkAmount)
        public
        pure
        returns (uint256)
    {
        return darkAmount / DARK_TO_USDT_RATE;
    }

    function buyWithBNB() external payable {
        require(!_locked, "Reentrant call");

        _locked = true;
        require(
            msg.value >= MIN_DARK_AMOUNT / DARK_TO_BNB_RATE,
            "Minimum BNB amount not met"
        );
        require(
            block.timestamp >= startTime && block.timestamp <= endTime,
            "Presale period has ended"
        );
        uint256 darkAmount = msg.value * DARK_TO_BNB_RATE;
        require(
            totalSoldDark + darkAmount <= TOTAL_DARK_SUPPLY,
            "Not enough DARK tokens left for sale"
        );
        darkBalances[msg.sender] += darkAmount;
        totalSoldDark += darkAmount;
        emit TokensPurchased(msg.sender, darkAmount);
    }

    function buyWithUSDT(uint256 usdtAmount) external {
        require(!_locked, "Reentrant call");

        _locked = true;
        require(
            usdtAmount >= MIN_DARK_AMOUNT / DARK_TO_USDT_RATE,
            "Minimum USDT amount not met"
        );
        require(
            block.timestamp >= startTime && block.timestamp <= endTime,
            "Presale period has ended"
        );
        uint256 darkAmount = calculateDarkAmount(usdtAmount);
        require(
            totalSoldDark + darkAmount <= TOTAL_DARK_SUPPLY,
            "Not enough DARK tokens left for sale"
        );
        IERC20(usdtTokenAddress).transferFrom(
            msg.sender,
            address(this),
            usdtAmount
        );
        darkBalances[msg.sender] += darkAmount;
        totalSoldDark += darkAmount;
        emit TokensPurchased(msg.sender, darkAmount);
        _locked = false;
    }


    function enableTrading(bool _tradingEnabled) external onlyOwner
    {
        tradingEnabled = _tradingEnabled;
        vestingStartTime = block.timestamp;
    }

    function claimTokensAfterPresale() external 
    {
        require(!_locked, "Reentrant call");
        require(block.timestamp >= endTime, "Presale has not ended yet");
        _locked = true;
         uint256 userBalance = darkBalances[msg.sender];
        uint256 tokensToRelease = (userBalance * 10) / 100;
        require(tokensToRelease > 0, "No tokens to release at the moment.");
        darkBalances[msg.sender] -= tokensToRelease;
        require(
            IERC20(darkTokenAddress).transfer(msg.sender, tokensToRelease),
            "Token transfer failed"
        );
        _locked = false;
         
    }

    function claimVestedTokens() external {
        require(!_locked, "Reentrant call");
        require(tradingEnabled,"Trading is not enabled yet");

        _locked = true;
        require(
            block.timestamp >= startTime + VESTING_INTERVAL,
            "Vesting period has not started yet"
        );
        require(block.timestamp >= endTime, "Presale has not ended yet");
        uint256 monthsVested = (block.timestamp - vestingStartTime) / VESTING_INTERVAL;
        
        uint256 vestingPercentage = 0;
        if (monthsVested <= 4) {
          
            vestingPercentage = 20; // 20% on month 2, month 3, month 4
        } else if (monthsVested == 5) {
          
            vestingPercentage = 20; // 20% on month 5
        } else if (monthsVested == 6) {
            
            vestingPercentage = 10; // 10% on month 6
        }
        uint256 userBalance = darkBalances[msg.sender];
        uint256 tokensToRelease = (userBalance * vestingPercentage) / 100;
        require(tokensToRelease > 0, "No tokens to release at the moment.");
        darkBalances[msg.sender] -= tokensToRelease;
        require(
            IERC20(darkTokenAddress).transfer(msg.sender, tokensToRelease),
            "Token transfer failed"
        );
        _locked = false;
    }

    function withdrawUnsoldTokens() external onlyOwner {
        require(block.timestamp > endTime, "Presale is still active");
        uint256 unsoldAmount = TOTAL_DARK_SUPPLY - totalSoldDark;
        IERC20(darkTokenAddress).transfer(owner, unsoldAmount);
    }

    function setPresalePeriod(uint256 _startTime, uint256 _endTime)
        external
        onlyOwner
    {
        require(_endTime > _startTime, "Invalid presale period");
        startTime = _startTime;
        endTime = _endTime;
    }

    function withdrawBNBAndUSDT() external {
        require(
            msg.sender == owner,
            "PaymentContract: Only owner can withdraw funds."
        );
        uint256 balance = address(this).balance;
        if (balance > 0) {
            owner.transfer(balance);
        }
        uint256 usdtBalance = IERC20(usdtTokenAddress).balanceOf(address(this));
        if (usdtBalance > 0) {
            bool success = IERC20(usdtTokenAddress).transfer(
                owner,
                usdtBalance
            );
            require(
                success,
                "PaymentContract: Failed to transfer USDT tokens."
            );
        }
    }
}