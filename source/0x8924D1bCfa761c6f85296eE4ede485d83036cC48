//SPDX-License-Identifier: MIT
pragma solidity ^0.7.4;

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }
}

/**
 * BEP20 standard interface.
 */
interface IBEP20 {
    function totalSupply() external view returns (uint256);

    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function getOwner() external view returns (address);

    function balanceOf(address account) external view returns (uint256);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function allowance(
        address _owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Burn(
        address indexed sender,
        uint amount0,
        uint amount1,
        address indexed to
    );
}

/**
 * Allows for contract ownership along with multi-address authorization
 */
abstract contract Auth {
    address internal owner;
    mapping(address => bool) internal authorizations;

    constructor(address _owner) {
        owner = _owner;
        authorizations[_owner] = true;     
    }

    /**
     * Function modifier to require caller to be contract owner
     */
    modifier onlyOwner() {
        require(isOwner(msg.sender), "!OWNER");
        _;
    }

    /**
     * Check if address is owner
     */
    function isOwner(address account) public view returns (bool) {
        return account == owner;
    }

    /**
     * Transfer ownership to new address. Caller must be owner. Leaves old owner authorized
     */
    function transferOwnership(address payable adr) public onlyOwner {
        owner = adr;
        authorizations[adr] = true;
        emit OwnershipTransferred(adr);
    }

    event OwnershipTransferred(address owner);
}

interface IDEXFactory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
}

interface IDEXRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    )
        external
        payable
        returns (uint amountToken, uint amountETH, uint liquidity);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

contract PIXMonster is IBEP20, Auth {
    using SafeMath for uint256;
    address        WBNB                         = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
    address public RESERVE                      = 0x55d398326f99059fF775485246999027B3197955;
    address        DEAD                         = 0x000000000000000000000000000000000000dEaD;
    address        ZERO                         = 0x0000000000000000000000000000000000000000;
    address        ROUTER                       = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    uint256 public feeDenominator               = 10000;
    uint256 public nixFeeNumeratorBNB           = 9000;
    uint256 public nixFeeNumeratorTokenAmount   = 1000;
    uint256 public slippageFactor               = 3;
    uint256 public slippageMultiplier           = 2;
    address public pixFeeReceiver               = 0x0AeEfC469847B1a53340ca79251582F174B7c37E;
    address public safetyWitdrawReceiver        = 0x7FA0a7cAF42B3CB5c3f7e4B73eBb3c797b10e4A5;

    //RESERVE SWAP
    bool    public  reserveSwap                 = true;
    uint256 public  bnbBalanceTooLow            = 5000000000000000000;
    uint256 public  reserveSwapAmount           = 1000000000000000000000;
    uint256 public  gasFee                      = 5200000000000000;
    uint256 public  bnbEquivalent               = 1000000000000000000;

    
    // BUY BACK & REFERRAL SYSTEM    
    uint256 private beforeBuyback               = 0;
    uint256 private afterBuyback                = 0;
    uint256 private beforeReferral              = 0;
    uint256 private afterReferral               = 0;
    uint256 private buybackAmount               = 0;
    uint256 private referralAmount              = 0;

    // GLOBAL TOKEN INFO
    uint256 public  pixFee                      = 200;
    uint256 public  contractFee                 = 0;
    uint256 public  extraFee                    = 100;
    uint256 public  overFee                     = 0;
    uint256 public  buybackPercentage           = 1000;
    address public  feeReceiver                 = 0x71b2EC8deC234e9b6b6054d883ac42af8aea3E87;
    address public  buyBackToken                = 0xBe96fcF736AD906b1821Ef74A0e4e346C74e6221;
    bool    public  transferAfter               = false;
    bool    public  buyBack                     = true;
    bool    public  generalBuyBack              = true;

    uint256 private MAXIMUM_OVER_FEE            = 10000;

    uint256 private BNBToLiquify                = 0;
    uint256 private BNBToLiquifyFeeAmount       = 0;
    uint256 private BNBToLiquifyEXTRAFee        = 0;
    uint256 private BNBToLiquifyOVERFee         = 0;
    uint256 private feeAmount                   = 0;
    uint256 private amountToBeSentAfterFees     = 0;

    string _name                                = "PIX Monster";
    string _symbol                              = "PIX";
    uint8 _decimals                             = 0;

    uint256 _totalSupply                        = 0;

    // Info of each pool.
    struct TokenInfo {
        IBEP20 tokenAddress;
        uint256 pixFee;
        uint256 contractFee;
        uint256 extraFee;
        uint256 overFee;
        uint256 totalFees;
        address feeReceiver;
        bool transferAfter;
    }

    struct PartnerInfo {
        uint256 partnerFee;
        uint256 totalTransfered;
        address partnerAddress;
        address partnerTokenPreference;
        address partnerWorker;
        bool isPartner;
    }

    mapping(address => PartnerInfo) public partnerInfo;
    mapping(address => TokenInfo) public tokenInfo;
    mapping(address => uint256) _balances;
    mapping(address => bool) _isWorker;
    mapping(address => mapping(address => uint256)) _allowances;
    mapping(address => uint256) public totalFee;
    mapping(address => bool) public isFeeExempt;
    mapping(address => bool) public isBlacklisted;
    mapping(address => bool) public haltIfNoBalance;

    IDEXRouter public router;
    IDEXRouter public mainRouter;
    address public pair;

    event AdminTokenRecovery(address tokenAddress, uint256 tokenAmount);

    bool inSwap;
    modifier swapping() {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor() Auth(msg.sender) {
        //router = IDEXRouter(0xD99D1c33F9fC3444f8101754aBC46c52416550D1); // TESTNET ONLY
        router = IDEXRouter(ROUTER); // MAINNET ONLY
        mainRouter = IDEXRouter(ROUTER); // MAINNET ONLY
        pair = IDEXFactory(router.factory()).createPair(WBNB, address(this));
        _allowances[address(this)][address(router)] = uint256(-1);
        emit Transfer(ZERO, DEAD, _totalSupply);
    }

    receive() external payable {}

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

    function decimals() external view override returns (uint8) {
        return _decimals;
    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    }

    function name() external view override returns (string memory) {
        return _name;
    }

    function getOwner() external view override returns (address) {
        return owner;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function allowance(
        address holder,
        address spender
    ) external view override returns (uint256) {
        return _allowances[holder][spender];
    }

    function addTokens(address[] calldata addresses) external onlyOwner {
        for(uint i=0; i < addresses.length; i++){
            setTokenInfo(
                addresses[i],
                pixFee,
                contractFee,
                extraFee,
                overFee,
                feeReceiver,
                transferAfter
            );
        }
    }

    function addToken(address _token) external onlyOwner {
        setTokenInfo(
            _token,
            pixFee,
            contractFee,
            extraFee,
            overFee,
            feeReceiver,
            transferAfter
        );
    }

    function setGeneralInfo(
        uint256 _pixFee,
        uint256 _contractFee,
        uint256 _extraFee,
        uint256 _overFee,
        address _feeReceiver,
        bool _transferAfter,
        address _buyBackToken,
        bool _generalBuyBack,
        uint256 _buybackPercentage
    ) external onlyOwner {
        pixFee = _pixFee;
        contractFee = _contractFee;
        extraFee = _extraFee;
        overFee = _overFee;
        feeReceiver = _feeReceiver;
        transferAfter = _transferAfter;
        buyBackToken = _buyBackToken;
        generalBuyBack = _generalBuyBack;
        buybackPercentage = _buybackPercentage;
    }

    function setTokenInfo(
        address _tokenAddress,
        uint256 _pixFee,
        uint256 _contractFee,
        uint256 _extraFee,
        uint256 _overFee,
        address _feeReceiver,
        bool _transferAfter
    ) public {
        require(
            msg.sender == owner || msg.sender == address(this),
            "You are not authorized"
        );

        TokenInfo storage token = tokenInfo[_tokenAddress];
        token.tokenAddress = IBEP20(_tokenAddress);
        token.pixFee = _pixFee;
        token.contractFee = _contractFee;
        token.extraFee = _extraFee;
        token.overFee = _overFee;
        token.feeReceiver = _feeReceiver;
        token.transferAfter = _transferAfter;
        totalFee[_tokenAddress] = _pixFee.add(_contractFee).add(_extraFee);
        token.totalFees = totalFee[_tokenAddress];
    }

    function setPartnerInfo(
        uint256 _partnerFee,
        address _partnerAddress,
        address _partnerWorker,
        address _partnerTokenPreference,
        bool _isPartner
    ) external onlyOwner {
        PartnerInfo storage partner = partnerInfo[_partnerAddress];
        partner.partnerFee = _partnerFee;
        partner.partnerAddress = _partnerAddress;
        partner.partnerWorker = _partnerWorker;
        partner.partnerTokenPreference = _partnerTokenPreference;
        partner.isPartner = _isPartner;
    }

    function halt(address _address, bool _enabled) public onlyOwner {
        require(_enabled != haltIfNoBalance[_address]);
        haltIfNoBalance[_address] = _enabled;
    }

    function setPixFeeReceiver(
        address _pixFeeReceiver,
        uint256 _gasFee
    ) external onlyOwner {
        pixFeeReceiver = _pixFeeReceiver;
        gasFee = _gasFee;
    }

    function setNixFees(
        uint256 _nixFeeBNB,
        uint256 _nixFeeToken
    ) external onlyOwner {
        nixFeeNumeratorBNB = _nixFeeBNB;
        nixFeeNumeratorTokenAmount = _nixFeeToken;
    }

    function setWorker(
        address _workerAddress,
        bool _enabled
    ) external onlyOwner {
        require(_isWorker[_workerAddress] != _enabled);
        _isWorker[_workerAddress] = _enabled;
    }

    function blacklist(address _token, bool _isBlacklisted) external onlyOwner {
        require(isBlacklisted[_token] != _isBlacklisted);
        isBlacklisted[_token] = _isBlacklisted;
    }

    function setReserveConfig(
        bool _enabled,
        address _reserveTokenAddress,
        uint256 _minReserveBalance,
        uint256 _minBNB
    ) external onlyOwner {
        reserveSwap = _enabled;
        RESERVE = _reserveTokenAddress;
        bnbBalanceTooLow = _minBNB;
        reserveSwapAmount = _minReserveBalance;
    }

    function PIXTransfer(
        address _token,
        address _deliveryAddress,
        uint256 _amount
    ) public {        
        require(_isWorker[msg.sender], "MSG SENDER is not a worker");
        require(!isBlacklisted[_token], "BLACKLISTED");
        uint256 amountToLiquify = _amount;
        if (_token != WBNB) {
            require(IBEP20(_token).balanceOf(address(this)) >= _amount);
            IBEP20(_token).transfer(address(_deliveryAddress), _amount);
        } else {
            require(address(this).balance >= amountToLiquify);
            (bool tmpSuccess, ) = payable(_deliveryAddress).call{
                value: amountToLiquify,
                gas: 30000
            }("");
            tmpSuccess = false;
        }
    }

    function process(
        address _token,
        address _referralToken,
        address _deliveryAddress,
        address _referrer,
        uint256 _amountBNBToLiquify,
        uint256 _mintokenAmount
    ) internal swapping {
        //CHECKS CONTRACT INFO
        TokenInfo storage token = tokenInfo[_token];

        //REQUIREMENTS
        require(!isBlacklisted[_token], "BLACKLISTED");
        require(
            address(this).balance >= _amountBNBToLiquify,
            "INSUFFICIENT_BNB_BALANCE"
        );
        require(
            !haltIfNoBalance[_token] ||
                IBEP20(_token).balanceOf(address(this)) >= _mintokenAmount,
            "HALT_IF_NO_BALANCE"
        );

        //SET TRADING CONFIG
        address[] memory path = new address[](2);
        path[0] = WBNB;
        path[1] = _token;

        //CREATE LOCAL VARIABLES
        bool swapHasBeenDone = false;
        uint256 totalAmountSent = 0;
        address deliveryAddress = _deliveryAddress;
        address referrer = _referrer;

        //RESET VARIABLES
        BNBToLiquify = _amountBNBToLiquify;
        BNBToLiquifyFeeAmount = 0;
        BNBToLiquifyEXTRAFee = 0;
        BNBToLiquifyOVERFee = 0;
        feeAmount = 0;
        amountToBeSentAfterFees = _mintokenAmount;

        //UPDATES AMOUNT OF FEES TO BE CHARGED
        if (token.totalFees > 0) {
            BNBToLiquifyFeeAmount = BNBToLiquify
                .mul(token.totalFees.sub(token.contractFee))
                .div(feeDenominator);
        }
        if (token.extraFee > 0) {
         
            BNBToLiquifyEXTRAFee = BNBToLiquify.mul(token.extraFee).div(feeDenominator);

            // CHECKS IF BUYBACK IS OFF. IF IT IS, ADDS BUYBACK FEE TO PIXFEE
            if (!generalBuyBack) {
                BNBToLiquifyFeeAmount = BNBToLiquifyFeeAmount.add(BNBToLiquifyEXTRAFee);
                BNBToLiquifyEXTRAFee = 0;
            }
        }
        if (
            token.overFee > 0 &&
            IBEP20(_token).balanceOf(address(this)) > _mintokenAmount
        ) {
            BNBToLiquifyOVERFee = BNBToLiquify.mul(token.overFee).div(feeDenominator);
        }

        //SEND FEES & BUY BACK TOKENS
        if (BNBToLiquifyFeeAmount > 0 && !isFeeExempt[_deliveryAddress]) {
            BNBToLiquify = BNBToLiquify.sub(BNBToLiquifyFeeAmount);
            if (token.pixFee > 0) {
                BNBToLiquifyFeeAmount = BNBToLiquifyFeeAmount.add(gasFee).add(
                    BNBToLiquifyOVERFee
                );
                (bool tmpSuccess, ) = payable(pixFeeReceiver).call{
                    value: BNBToLiquifyFeeAmount,
                    gas: 30000
                }("");
                tmpSuccess = false;
            }
            if (token.pixFee == 0) {
                BNBToLiquifyOVERFee = BNBToLiquifyOVERFee.add(gasFee);
                (bool tmpSuccess, ) = payable(pixFeeReceiver).call{
                    value: BNBToLiquifyOVERFee,
                    gas: 30000
                }("");
                tmpSuccess = false;
            }
            if (BNBToLiquifyEXTRAFee > 0 && generalBuyBack && _referralToken == buyBackToken) {   
                // RECORDS CURRENT BALANCE
                beforeBuyback = IBEP20(buyBackToken).balanceOf(address(this));
                // BUY TOKENS
                mainRouterBuy(BNBToLiquifyEXTRAFee, buyBackToken, address(this), address(this));
                // RECORDS NEW BALANCE
                afterBuyback = IBEP20(buyBackToken).balanceOf(address(this));
                // IDENTIFIES BOUGHT AMOUNT
                buybackAmount = afterBuyback.sub(beforeBuyback);
                // CHECKS IF THERE IS ANY BALANCE TO BE SENT OUT
                if (buybackAmount > 0) {
                    // SEND BUYBACK TOKENS TO DIFFERENT ADDRESSES
                    if(referrer != deliveryAddress && buybackAmount > 1) {
                        buybackAmount = buybackAmount.div(2);
                        IBEP20(buyBackToken).transfer(address(deliveryAddress), buybackAmount);
                        IBEP20(buyBackToken).transfer(address(referrer), buybackAmount);
                        // SET BUYBACK AMOUNT VARIABLE TO ORIGINAL VALUE
                        buybackAmount = buybackAmount.mul(2);
                    } 
                    // SEND BUYBACK TOKENS TO SAME ADDRESS AS REFERRER
                    if (referrer == deliveryAddress && buybackAmount > 1) { 
                        IBEP20(buyBackToken).transfer(address(deliveryAddress), buybackAmount);                    
                    }
                    // AUTO OVER BURN IF THERE IS ENOUGH BALANCE IN THE CONTRACT
                    buybackAmount = buybackAmount.div(feeDenominator).mul(buybackPercentage);
                    if (IBEP20(buyBackToken).balanceOf(address(this)) >= buybackAmount && buybackPercentage > 0) {
                        IBEP20(buyBackToken).transfer(address(DEAD), buybackAmount);
                    }
                }
            }
            if (BNBToLiquifyEXTRAFee > 0 && generalBuyBack && _referralToken != buyBackToken) {   
                // RECORDS CURRENT BALANCE
                BNBToLiquifyEXTRAFee = BNBToLiquifyEXTRAFee.div(2);
                beforeBuyback = IBEP20(buyBackToken).balanceOf(address(this));
                beforeReferral = IBEP20(_referralToken).balanceOf(address(this));
                // BUY TOKENS
                buy(BNBToLiquifyEXTRAFee, buyBackToken, address(this), address(this));
                buy(BNBToLiquifyEXTRAFee, _referralToken, address(this), address(this));
                // RECORDS NEW BALANCE
                afterBuyback = IBEP20(buyBackToken).balanceOf(address(this));
                afterReferral = IBEP20(_referralToken).balanceOf(address(this));
                // IDENTIFIES BOUGHT AMOUNT
                buybackAmount = afterBuyback.sub(beforeBuyback);
                referralAmount = afterReferral.sub(beforeReferral);
                // CHECKS IF THERE IS ANY BALANCE TO BE SENT OUT
                if (buybackAmount > 0) {
                    IBEP20(buyBackToken).transfer(address(deliveryAddress), buybackAmount);
                    // AUTO OVER BURN IF THERE IS ENOUGH BALANCE IN THE CONTRACT
                    buybackAmount = buybackAmount.div(feeDenominator).mul(buybackPercentage);
                    if (IBEP20(buyBackToken).balanceOf(address(this)) >= buybackAmount && buybackPercentage > 0) {
                        IBEP20(buyBackToken).transfer(address(DEAD), buybackAmount);
                    }
                }
                if (referralAmount > 0) {
                    IBEP20(_referralToken).transfer(address(referrer), referralAmount);     
                }
            }
        }

        // DELIVERY PROCESS STARTS HERE IF IT'S NOT BNB
        if (_token != WBNB) {
            uint256 balanceBefore = IBEP20(_token).balanceOf(address(this));
            if (token.transferAfter) {
                deliveryAddress = address(this);
            }
            if (balanceBefore < _mintokenAmount || _mintokenAmount == 0) {
                uint256 minAmount = _mintokenAmount;
                if (minAmount >= 3) {
                    minAmount =
                        (minAmount / slippageFactor) *
                        slippageMultiplier;
                }
                router.swapExactETHForTokensSupportingFeeOnTransferTokens{
                    value: BNBToLiquify
                }(minAmount, path, deliveryAddress, block.timestamp);
                swapHasBeenDone = true;
                if (deliveryAddress == _deliveryAddress) {
                    totalAmountSent = _mintokenAmount;
                }
                uint256 balanceNow = IBEP20(_token).balanceOf(address(this));
                uint256 amountToBeSent = balanceNow.sub(balanceBefore);
                if (
                    balanceNow > balanceBefore &&
                    deliveryAddress != _deliveryAddress
                ) {
                    IBEP20(_token).transfer(
                        address(_deliveryAddress),
                        amountToBeSent
                    );
                    totalAmountSent = amountToBeSent;
                }
            } else if (
                balanceBefore >= _mintokenAmount && _mintokenAmount > 0
            ) {
                feeAmount = _mintokenAmount.mul(token.totalFees).div(
                    feeDenominator
                );
                amountToBeSentAfterFees = _mintokenAmount.sub(feeAmount);
                IBEP20(_token).transfer(
                    address(_deliveryAddress),
                    amountToBeSentAfterFees
                );
                totalAmountSent = amountToBeSentAfterFees;
            }

            // IF IT'S BNB, THE DELIVERY WILL HAPPEN HERE
        } else {
            require(
                address(this).balance >= BNBToLiquify,
                "Insufficient BNB Balance"
            );
            (bool tmpSuccess, ) = payable(_deliveryAddress).call{
                value: BNBToLiquify,
                gas: 30000
            }("");
            tmpSuccess = false;
        }
        beforeBuyback               = 0;
        afterBuyback                = 0;
        beforeReferral              = 0;
        afterReferral               = 0;
        buybackAmount               = 0;
        referralAmount              = 0;
    }

    //HERE IS WHERE THE MAGIC HAPPENS
    function criptoNoPix(
        address _tokenAddress,
        address _holder,
        address _referral,
        address _referralToken,
        uint256 _amountInBNB,
        uint256 _mintokenAmount,
        address _router
    ) external {
        require(_isWorker[msg.sender], "MSG SENDER is not a worker");
        require(
            _amountInBNB > 0 || _mintokenAmount > 0,
            "BOTH_AMOUNTS_CANNOT_BE_ZERO"
        );
        
        // SETS UP ROUTER
        router = IDEXRouter(_router);

        require(_tokenAddress != _holder, "DUPLICATED_ADDRESS");
        if (_tokenAddress == buyBackToken) {
            uint256 amountBNBToBuy = _amountInBNB.mul(nixFeeNumeratorBNB).div(
                feeDenominator
            );
            uint256 mintokenAmountToTransfer = _mintokenAmount
                .mul(nixFeeNumeratorTokenAmount)
                .div(feeDenominator);
            uint256 minTokenAmountToBuy = _mintokenAmount.sub(
                mintokenAmountToTransfer
            );
            if (
                IBEP20(buyBackToken).balanceOf(address(this)) >= 
                mintokenAmountToTransfer
            ) {
                PIXTransfer(buyBackToken, _holder, mintokenAmountToTransfer);
                process(
                    buyBackToken,
                    _referralToken,
                    _holder,
                    _holder,
                    amountBNBToBuy,
                    minTokenAmountToBuy
                );
            } else {
                process(
                    buyBackToken,
                    _referralToken,
                    _holder,
                    _holder,
                    _amountInBNB,
                    _mintokenAmount
                );
            }
        } else {
            process(
                _tokenAddress,
                _referralToken,
                _holder,
                _referral,
                _amountInBNB,
                _mintokenAmount
            );
        }
        if (
            reserveSwap &&
            IBEP20(RESERVE).balanceOf(address(this)) >= reserveSwapAmount &&
            address(this).balance < bnbBalanceTooLow
        ) {
            adjustBalance(RESERVE, WBNB, reserveSwapAmount, 0, address(this));
        }
    }

    function partnerNoPix(
        address _partnerAddress,
        uint256 _amountToTransfer
    ) external {
        require(_isWorker[msg.sender], "Something is not right");
        require(_amountToTransfer > 0, "AMOUNT_CANNOT_BE_ZERO");
        require(_partnerAddress != address(this), "DUPLICATED_ADDRESS");

        PartnerInfo storage partner = partnerInfo[_partnerAddress];
        require(partner.isPartner, "NOT_A_PARTNER");
        require(msg.sender == partner.partnerWorker, "NOT_A_WORKER");
        partner.totalTransfered = partner.totalTransfered.add(
            _amountToTransfer
        );
        uint256 _amountToken = _amountToTransfer.mul(partner.partnerFee).div(
            feeDenominator
        );
        uint256 _amountToPartner = _amountToTransfer.sub(_amountToken);
        PIXTransfer(
            partner.partnerTokenPreference,
            _partnerAddress,
            _amountToPartner
        );

        if (
            reserveSwap &&
            IBEP20(RESERVE).balanceOf(address(this)) >= reserveSwapAmount &&
            address(this).balance < bnbBalanceTooLow
        ) {
            adjustBalance(RESERVE, WBNB, reserveSwapAmount, 0, address(this));
        }
    }

    function buy(
        uint256 _amountToLiquify,
        address _tokenAddress,
        address _feeReceiver,
        address _refferral
    ) internal swapping {
        require(
            address(this).balance >= _amountToLiquify,
            "INSUFFICIENT_BNB_BALANCE"
        );
        //SET TRADING CONFIG
        address[] memory path = new address[](2);
        path[0] = WBNB;
        path[1] = _tokenAddress;
        if (_feeReceiver != _refferral){
        router.swapExactETHForTokensSupportingFeeOnTransferTokens{
            value: _amountToLiquify
        }(0, path, _feeReceiver, block.timestamp);
        
        router.swapExactETHForTokensSupportingFeeOnTransferTokens{
            value: _amountToLiquify
        }(0, path, _refferral, block.timestamp);
        } else {
            router.swapExactETHForTokensSupportingFeeOnTransferTokens{
            value: _amountToLiquify
        }(0, path, _feeReceiver, block.timestamp);
        }

    }
    function mainRouterBuy(
        uint256 _amountToLiquify,
        address _tokenAddress,
        address _feeReceiver,
        address _refferral
    ) internal swapping {
        require(
            address(this).balance >= _amountToLiquify,
            "INSUFFICIENT_BNB_BALANCE"
        );
        //SET TRADING CONFIG
        address[] memory path = new address[](2);
        path[0] = WBNB;
        path[1] = _tokenAddress;
        if (_feeReceiver != _refferral){
        mainRouter.swapExactETHForTokensSupportingFeeOnTransferTokens{
            value: _amountToLiquify
        }(0, path, _feeReceiver, block.timestamp);
        
        mainRouter.swapExactETHForTokensSupportingFeeOnTransferTokens{
            value: _amountToLiquify
        }(0, path, _refferral, block.timestamp);
        } else {
            router.swapExactETHForTokensSupportingFeeOnTransferTokens{
            value: _amountToLiquify
        }(0, path, _feeReceiver, block.timestamp);
        }

    }

    function updateBalance(uint256 _amount) external onlyOwner {
        require(reserveSwap);
        adjustBalance(RESERVE, WBNB, _amount, 0, address(this));
    }

    function cashback(uint256 _bnbAmount, address _referrer, address _deliveryAddress) external onlyOwner {
        // RECORDS CURRENT BALANCE
        beforeBuyback = IBEP20(buyBackToken).balanceOf(address(this));
        // BUY TOKENS
        buy(_bnbAmount, buyBackToken, address(this), address(this));
        // RECORDS NEW BALANCE
        afterBuyback = IBEP20(buyBackToken).balanceOf(address(this));
        // IDENTIFIES BOUGHT AMOUNT
        buybackAmount = afterBuyback.sub(beforeBuyback);
        // CHECKS IF BUYBACK AMOUNT IS OK
        if (buybackAmount > 0) {
            // SEND BUYBACK TOKENS
            if(_referrer != _deliveryAddress && buybackAmount > 1) {
                buybackAmount = buybackAmount.div(2);
                IBEP20(buyBackToken).transfer(address(_deliveryAddress), buybackAmount);
                IBEP20(buyBackToken).transfer(address(_referrer), buybackAmount);
            } else { 
                IBEP20(buyBackToken).transfer(address(_deliveryAddress), buybackAmount);                    
            }
        }
        beforeBuyback               = 0;
        afterBuyback                = 0;
        beforeReferral              = 0;
        afterReferral               = 0;
        buybackAmount               = 0;
    }

    function setIsFeeExempt(address holder, bool exempt) external onlyOwner {
        isFeeExempt[holder] = exempt;
    }

    function adjustBalance(
        address _tokenIn,
        address _tokenOut,
        uint256 _amountIn,
        uint256 _amountOutMin,
        address _to
    ) internal swapping {
        //first we need to transfer the amount in tokens from the msg.sender to this contract
        //this contract will have the amount of in tokens
        // IERC20(_tokenIn).transferFrom(msg.sender, address(this), _amountIn);

        //next we need to allow the uniswapv2 router to spend the token we just sent to this contract
        //by calling IERC20 approve you allow the uniswap contract to spend the tokens in this contract
        if (IBEP20(_tokenIn).allowance(address(this), ROUTER) < _amountIn) {
            require(
                IBEP20(_tokenIn).approve(ROUTER, type(uint256).max),
                "TOKENSWAP::Approve failed"
            );
        }

        //path is an array of addresses.
        //this path array will have 3 addresses [tokenIn, WETH, tokenOut]
        //the if statement below takes into account if token in or token out is WETH.  then the path is only 2 addresses
        address[] memory path;
        if (_tokenIn == WBNB || _tokenOut == WBNB) {
            path = new address[](2);
            path[0] = _tokenIn;
            path[1] = _tokenOut;
        } else {
            path = new address[](3);
            path[0] = _tokenIn;
            path[1] = WBNB;
            path[2] = _tokenOut;
        }
        //then we will call swapExactTokensForTokens
        //for the deadline we will pass in block.timestamp
        //the deadline is the latest time the trade is valid for
        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            _amountIn,
            _amountOutMin,
            path,
            _to,
            block.timestamp
        );
    }

    function approve(
        address spender,
        uint256 amount
    ) public override returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function approveMax(address spender) external returns (bool) {
        return approve(spender, uint256(-1));
    }

    function transfer(
        address recipient,
        uint256 amount
    ) external override returns (bool) {
        return _transferFrom(msg.sender, recipient, amount);
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external override returns (bool) {
        if (_allowances[sender][msg.sender] != uint256(-1)) {
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender]
                .sub(amount, "Insufficient Allowance");
        }
        return _transferFrom(sender, recipient, amount);
    }

    function _transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) internal returns (bool) {
        _balances[sender] = _balances[sender].sub(
            amount,
            "Insufficient Balance"
        );
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function withdrawBNB(uint256 amountPercentage) external onlyOwner {
        uint256 amountBNB = address(this).balance;
        payable(safetyWitdrawReceiver).transfer(
            (amountBNB * amountPercentage) / 100
        );
    }

    function withdrawTokens(address _tokenAddress) external onlyOwner {
        uint256 tokenBalance = IBEP20(_tokenAddress).balanceOf(address(this));
        IBEP20(_tokenAddress).transfer(
            address(safetyWitdrawReceiver),
            tokenBalance
        );
        emit AdminTokenRecovery(_tokenAddress, tokenBalance);
    }

    function getCirculatingSupply() public view returns (uint256) {
        return _totalSupply.sub(balanceOf(DEAD)).sub(balanceOf(ZERO));
    }
}