/*                                                                                                                                                                                      
 * ARK Terra Card BSC
 * 
 * Written by: MrGreenCrypto
 * Co-Founder of CodeCraftrs.com
 * 
 * SPDX-License-Identifier: None
 */

pragma solidity 0.8.19;

interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address _owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount ) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IDEXRouter {
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline) external;
    function getAmountsOut(uint256 amountIn, address[] calldata path) external view returns (uint256[] memory amounts);
    function swapExactETHForTokensSupportingFeeOnTransferTokens(uint256 amountOutMin, address[] calldata path, address to, uint256 deadline) external payable;
}

interface ILEGACY {
    function balanceOf(address investor) external view returns (uint256);
}

contract CC_OFFRAMP  {
    address public constant CEO = 0xdf0048DF98A749ED36553788B4b449eA7a7BAA88;
    address public treasury = 0x0de231ceb05E7A3f3Cc70387Cc9ECCb75842E07f;
    address public fallbackReferrer = 0x0de231ceb05E7A3f3Cc70387Cc9ECCb75842E07f;
    address public legacyAddress = 0x6733a95654fF739e6058247Dd7556BF9Ce2dEdf6;
    
    IBEP20 public constant USDT = IBEP20(0x55d398326f99059fF775485246999027B3197955);
    IDEXRouter public constant ROUTER = IDEXRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
    address public constant WBNB = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
    
    mapping(string => Deposit) public deposits;
    mapping(string => CardBuy) public cardBuys;
    mapping(address => address) public referrerOf;
    mapping(address => address[]) public downline;   
    mapping(address => uint256) public totalReferralRewards1;
    mapping(address => uint256) public totalReferralRewards2;
    mapping(address => uint256) public totalReferralRewardsCard1;
    mapping(address => uint256) public totalReferralRewardsCard2;
    mapping(address => uint256) public totalReferrals;

    event DepositDone(string uid, Deposit details);
    event CardBought(string uid, CardBuy details);
    
    uint256 multiplier = 1 ether;
    uint256 minDeposit;
    uint256 maxDeposit = 5000 * multiplier;
    uint256 public affiliateReward = 20;
    uint256 public secondLevelReward = 5;
    uint256 public rewardDivisor = 10000;

    uint256 public plasticFirstLevel = 25 * multiplier;
    uint256 public plasticSecondLevel = 5 * multiplier;
    uint256 public metalFirstLevel = 40 * multiplier;
    uint256 public metalSecondLevel = 10* multiplier;
    uint256 public customFirstLevel = 65 * multiplier;
    uint256 public customSecondLevel = 15 * multiplier;
    uint256 public customPrice = 400 * multiplier;
    uint256 public metalPrice = 250 * multiplier;
    uint256 public plasticPrice = 150 * multiplier;

    struct Deposit {
        address user;
        address currency;
        uint256 currencyAmount;
        uint256 depositAmount;
        uint256 timestamp;
        bool isReload;
        bool hasSupport;
        bool hasLegacy;
        address referrer;
        address secondLevelReferrer;
    }

    struct CardBuy {
        address user;
        address currency;
        uint256 currencyAmount;
        uint256 depositAmount;
        uint256 timestamp;
        bool isMetal;
        bool isCustom;
        bool hasLegacy;
        address referrer;
        address secondLevelReferrer;
    }

    modifier onlyCEO() {
        require(msg.sender == CEO, "Only CEO");
        _;
    }

	constructor() {
        USDT.approve(address(ROUTER), type(uint256).max);
    }

    receive() external payable {}

    function checkIfUidIsUsed(string memory uid) internal view returns (bool) {
        if(deposits[uid].timestamp != 0) return true;
        return false;
    }

    function depositMoneyUSDT(uint256 amount, string memory uid, bool isReload, bool hasSupport, address referrer) external {
        require(!checkIfUidIsUsed(uid),"Uid already exists");
        uint256 balanceBefore = USDT.balanceOf(address(this));
        require(USDT.transferFrom(msg.sender, address(this), amount), "failed");
        Deposit memory deposit = Deposit(msg.sender, address(USDT), amount, 0, block.timestamp, isReload, hasSupport, false, address(0), address(0));
        deposits[uid] = deposit;
        if(referrer == address(0) || referrer == msg.sender) referrer = fallbackReferrer;  
        if(referrerOf[msg.sender] == address(0)) {
            referrerOf[msg.sender] = referrer;
            downline[referrer].push(msg.sender);
        }
        _deposit(balanceBefore, uid, referrerOf[msg.sender]);
    }

    function depositMoneyBNB(string memory uid, uint256 minOut, bool isReload, bool hasSupport, address referrer) public payable {
        require(!checkIfUidIsUsed(uid),"Uid already exists");
        uint256 balanceBefore = USDT.balanceOf(address(this));
        Deposit memory deposit = Deposit(msg.sender, address(0), msg.value, 0, block.timestamp, isReload, hasSupport, false, address(0), address(0));
        deposits[uid] = deposit;

        address[] memory path = new address[](2);
        path[0] = WBNB;
        path[1] = address(USDT);
        
        ROUTER.swapExactETHForTokensSupportingFeeOnTransferTokens{value: msg.value}(
            minOut,
            path,
            address(this),
            block.timestamp
        );
        if(referrer == address(0) || referrer == msg.sender) referrer = fallbackReferrer;  
        if(referrerOf[msg.sender] == address(0)) {
            referrerOf[msg.sender] = referrer;
            downline[referrer].push(msg.sender);
        }
        _deposit(balanceBefore, uid, referrerOf[msg.sender]);
    }

    function depositMoneyEasy(
        uint256 amount, 
        address currency, 
        uint256 minOut, 
        string memory uid, 
        bool isReload, 
        bool hasSupport, 
        address referrer
    ) external {
        require(!checkIfUidIsUsed(uid),"Uid already exists");
        require(IBEP20(currency).transferFrom(msg.sender, address(this), amount), "failed");
        IBEP20(currency).approve(address(ROUTER), type(uint256).max);

        Deposit memory deposit = Deposit(msg.sender, currency, amount, 0, block.timestamp, isReload, hasSupport, false, address(0), address(0));
        deposits[uid] = deposit;

        address[] memory path = new address[](2);
        path[0] = currency;
        path[1] = address(USDT);

        uint256 balanceBefore = USDT.balanceOf(address(this));
        ROUTER.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            amount,
            minOut,
            path,
            address(this),
            block.timestamp
        );
        if(referrer == address(0) || referrer == msg.sender) referrer = fallbackReferrer;  
        if(referrerOf[msg.sender] == address(0)) {
            referrerOf[msg.sender] = referrer;
            downline[referrer].push(msg.sender);
        }
        _deposit(balanceBefore, uid, referrerOf[msg.sender]);
    }

    function depositMoneyExpert(
        uint256 amount,
        address[] memory path,
        uint256 minOut,
        string memory uid,
        bool isReload,
        bool hasSupport,
        address referrer
    ) external {
        require(!checkIfUidIsUsed(uid),"Uid already exists");
        require(IBEP20(path[0]).transferFrom(msg.sender, address(this), amount), "failed");
        require(path[path.length - 1] == address(USDT), "wrong");
        IBEP20(path[0]).approve(address(ROUTER), type(uint256).max);

        Deposit memory deposit = Deposit(msg.sender, path[0], amount, 0, block.timestamp, isReload, hasSupport, false, address(0), address(0));
        deposits[uid] = deposit;
        
        uint256 balanceBefore = USDT.balanceOf(address(this));
        ROUTER.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            amount,
            minOut,
            path,
            address(this),
            block.timestamp
        );
        if(referrer == address(0) || referrer == msg.sender) referrer = fallbackReferrer;  
        if(referrerOf[msg.sender] == address(0)) {
            referrerOf[msg.sender] = referrer;
            downline[referrer].push(msg.sender);
        }
        _deposit(balanceBefore, uid, referrerOf[msg.sender]);
    }

    function _deposit(uint256 balanceBefore, string memory uid, address referrer) internal {
        uint256 depositAmount = USDT.balanceOf(address(this)) - balanceBefore;
        require(depositAmount >= minDeposit, "Min deposit");
        require(depositAmount <= maxDeposit, "Max deposit");
        deposits[uid].depositAmount = depositAmount;
        bool hasLegacy = ILEGACY(legacyAddress).balanceOf(msg.sender) > 0;
        deposits[uid].hasLegacy = hasLegacy;
        if(!hasLegacy) {
            uint256 firstLevelAmount = depositAmount * affiliateReward / rewardDivisor;
            uint256 secondLevelAmount = depositAmount * secondLevelReward / rewardDivisor;
            
            require(USDT.transfer(referrer, firstLevelAmount), "failed");
            if(referrerOf[referrer] == address(0)) referrerOf[referrer] = fallbackReferrer;
            address secondLevelAddress = referrerOf[referrer];
            require(USDT.transfer(secondLevelAddress, secondLevelAmount), "failed");
            totalReferralRewards1[referrer] += firstLevelAmount;
            totalReferralRewards2[secondLevelAddress] += secondLevelAmount;
            totalReferrals[referrer]++;
            totalReferrals[secondLevelAddress]++;
            deposits[uid].referrer = referrer;
            deposits[uid].secondLevelReferrer = secondLevelAddress;
            depositAmount -= firstLevelAmount;
            depositAmount -= secondLevelAmount;
        }

        require(USDT.transfer(treasury, depositAmount), "failed");
        emit DepositDone(uid, deposits[uid]);
    }

    function expectedUSDTFromCurrency(uint256 input, address currency) public view returns(uint256) {
        address[] memory path = new address[](2);
        path[0] = currency;
        path[1] = address(USDT);
        uint256 usdtAmount = ROUTER.getAmountsOut(input, path)[path.length - 1];
        return usdtAmount; 
    }

    function expectedUSDTFromPath(uint256 input, address[] memory path) public view returns(uint256) {
        require(path[path.length-1] == address(USDT), "USDT");
        uint256 usdtAmount = ROUTER.getAmountsOut(input, path)[path.length - 1];
        return usdtAmount;
    }

    function rescueAnyToken(IBEP20 tokenToRescue) external onlyCEO {
        uint256 _balance = tokenToRescue.balanceOf(address(this));
        tokenToRescue.transfer(CEO, _balance);
    }

    function rescueBnb() external onlyCEO {
        (bool success,) = address(CEO).call{value: address(this).balance}("");
        if(success) return;
    } 

    function setLimits(uint256 newMinDeposit, uint256 newMaxDeposit) external onlyCEO {
        minDeposit = newMinDeposit * multiplier;
        maxDeposit = newMaxDeposit * multiplier;
    }
    
    function setTreasury(address newTreasury) external onlyCEO {
        treasury = newTreasury;
    }    

    function setLegacy(address newLegacy) external onlyCEO {
        legacyAddress = newLegacy;
    }

    function setFallbackReferrer(address newFallbackReferrer) external onlyCEO {
        fallbackReferrer = newFallbackReferrer;
    }

    function setReferrer(address investor, address newReferrer) external onlyCEO {
        referrerOf[investor] = newReferrer;
    }

    function setReferrers(address oldReferrer, address newReferrer) external onlyCEO {
        for(uint256 i = 0; i<downline[oldReferrer].length;i++){
            referrerOf[downline[oldReferrer][i]] = newReferrer;
        }
    }

    function setAffiliateReward(uint256 newAffiliateReward, uint256 newSecondLevelReward) external onlyCEO {
        affiliateReward = newAffiliateReward * multiplier;
        secondLevelReward = newSecondLevelReward * multiplier;
    }


    function buyCardUSDT(uint256 amount, string memory uid, bool isMetal, bool isCustom, address referrer) external {
        require(!checkIfUidIsUsed(uid),"Uid already exists");
        uint256 balanceBefore = USDT.balanceOf(address(this));
        require(USDT.transferFrom(msg.sender, address(this), amount), "failed");
        CardBuy memory buy = CardBuy(msg.sender, address(USDT), amount, 0, block.timestamp, isMetal, isCustom, false, address(0), address(0));
        cardBuys[uid] = buy;
        if(referrer == address(0) || referrer == msg.sender) referrer = fallbackReferrer;  
        if(referrerOf[msg.sender] == address(0)) {
            referrerOf[msg.sender] = referrer;
            downline[referrer].push(msg.sender);
        }
        _cardBuy(balanceBefore, uid, referrerOf[msg.sender]);
    }

    function buyCardBNB(string memory uid, uint256 minOut, bool isMetal, bool isCustom, address referrer) public payable {
        require(!checkIfUidIsUsed(uid),"Uid already exists");
        uint256 balanceBefore = USDT.balanceOf(address(this));
        CardBuy memory buy = CardBuy(msg.sender, address(0), msg.value, 0, block.timestamp, isMetal, isCustom, false, address(0), address(0));
        cardBuys[uid] = buy;

        address[] memory path = new address[](2);
        path[0] = WBNB;
        path[1] = address(USDT);
        
        ROUTER.swapExactETHForTokensSupportingFeeOnTransferTokens{value: msg.value}(
            minOut,
            path,
            address(this),
            block.timestamp
        );
        if(referrer == address(0) || referrer == msg.sender) referrer = fallbackReferrer;  
        if(referrerOf[msg.sender] == address(0)) {
            referrerOf[msg.sender] = referrer;
            downline[referrer].push(msg.sender);
        }
        _cardBuy(balanceBefore, uid, referrerOf[msg.sender]);
    }

    function buyCardEasy(
        uint256 amount, 
        address currency, 
        uint256 minOut, 
        string memory uid, 
        bool isMetal,
        bool isCustom,
        address referrer
    ) external {
        require(!checkIfUidIsUsed(uid),"Uid already exists");
        require(IBEP20(currency).transferFrom(msg.sender, address(this), amount), "failed");
        IBEP20(currency).approve(address(ROUTER), type(uint256).max);

        CardBuy memory buy = CardBuy(msg.sender, currency, amount, 0, block.timestamp, isMetal, isCustom, false, address(0), address(0));
        cardBuys[uid] = buy;

        address[] memory path = new address[](2);
        path[0] = currency;
        path[1] = address(USDT);

        uint256 balanceBefore = USDT.balanceOf(address(this));
        ROUTER.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            amount,
            minOut,
            path,
            address(this),
            block.timestamp
        );
        if(referrer == address(0) || referrer == msg.sender) referrer = fallbackReferrer;  
        if(referrerOf[msg.sender] == address(0)) {
            referrerOf[msg.sender] = referrer;
            downline[referrer].push(msg.sender);
        }
        _cardBuy(balanceBefore, uid, referrerOf[msg.sender]);
    }

    function buyCardExpert(
        uint256 amount,
        address[] memory path,
        uint256 minOut,
        string memory uid,
        bool isMetal,
        bool isCustom,
        address referrer
    ) external {
        require(!checkIfUidIsUsed(uid),"Uid already exists");
        require(IBEP20(path[0]).transferFrom(msg.sender, address(this), amount), "failed");
        require(path[path.length - 1] == address(USDT), "wrong");
        IBEP20(path[0]).approve(address(ROUTER), type(uint256).max);

        CardBuy memory buy = CardBuy(msg.sender, path[0], amount, 0, block.timestamp, isMetal, isCustom, false, address(0), address(0));
        cardBuys[uid] = buy;
        
        uint256 balanceBefore = USDT.balanceOf(address(this));
        ROUTER.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            amount,
            minOut,
            path,
            address(this),
            block.timestamp
        );
        if(referrer == address(0) || referrer == msg.sender) referrer = fallbackReferrer;  
        if(referrerOf[msg.sender] == address(0)) {
            referrerOf[msg.sender] = referrer;
            downline[referrer].push(msg.sender);
        }
        _cardBuy(balanceBefore, uid, referrerOf[msg.sender]);
    }


    function _cardBuy(uint256 balanceBefore, string memory uid, address referrer) internal {
        uint256 depositAmount = USDT.balanceOf(address(this)) - balanceBefore;
        if(cardBuys[uid].isCustom) require(depositAmount >= customPrice, "Not enough");
        else if(cardBuys[uid].isMetal) require(depositAmount >= metalPrice, "Not enough");
        else require(depositAmount >= plasticPrice, "Not enough");

        cardBuys[uid].depositAmount = depositAmount;
        bool hasLegacy = ILEGACY(legacyAddress).balanceOf(msg.sender) > 0;
        cardBuys[uid].hasLegacy = hasLegacy;

        uint256 firstLevelAmount = cardBuys[uid].isCustom ? customFirstLevel : cardBuys[uid].isMetal ? metalFirstLevel : plasticFirstLevel;
        uint256 secondLevelAmount = cardBuys[uid].isCustom ? customSecondLevel : cardBuys[uid].isMetal ? metalSecondLevel : plasticSecondLevel;
            
        require(USDT.transfer(referrer, firstLevelAmount), "failed");
        if(referrerOf[referrer] == address(0)) referrerOf[referrer] = fallbackReferrer;
        address secondLevelAddress = referrerOf[referrer];
        require(USDT.transfer(secondLevelAddress, secondLevelAmount), "failed");
        totalReferralRewardsCard1[referrer] += firstLevelAmount;
        totalReferralRewardsCard2[secondLevelAddress] += secondLevelAmount;
        totalReferrals[referrer]++;
        totalReferrals[secondLevelAddress]++;
        deposits[uid].referrer = referrer;
        deposits[uid].secondLevelReferrer = secondLevelAddress;
        depositAmount -= firstLevelAmount;
        depositAmount -= secondLevelAmount;

        require(USDT.transfer(treasury, depositAmount), "failed");
        emit CardBought(uid, cardBuys[uid]);
    }
}