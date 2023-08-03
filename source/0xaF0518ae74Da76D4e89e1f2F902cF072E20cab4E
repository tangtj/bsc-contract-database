pragma solidity 0.8.16;
// SPDX-License-Identifier: Unlicensed

//PANCAKE FACTORY
interface PancakeSwapFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

// PANCAKE ROUTER
interface PancakeSwapRouter {
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
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

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

// IBEP20
interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function getOwner() external view returns (address);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

// SAFEMATH
library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {return 0;}
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this;
        return msg.data;
    }
}

// OWNABLE
contract Ownable is Context {
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    mapping (address => bool) internal authorizations;
    address private owner;

    constructor () {
        address msgSender = _msgSender();
        owner = msgSender;
        authorizations[owner] = true;
        emit OwnershipTransferred(address(0), msgSender);
    }

    modifier onlyOwner() {
        require(owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function _owner() public view returns (address) {
        return owner;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(owner, address(0));
        owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }
}
// =======================================================================================================================
// TOKEN
contract WackyBucks is Ownable, IBEP20 {
    using SafeMath for uint256;

    // Token information
    string constant private Name = "Wacky Bucks";
    string constant private Symbol = "WACKY";
    uint8 constant private Decimals = 18;
    uint256 private TotalSupply = 1000000 * (10 ** Decimals);

    // Max transaction and wallet limits
    uint256 public TransactionMax = TotalSupply * 20 / 1000;
    uint256 public WalletMax = TotalSupply * 50 / 1000;

    // Special wallet addresses
    address private DEAD_WALLET = 0x000000000000000000000000000000000000dEaD;
    address private ZERO_WALLET = 0x0000000000000000000000000000000000000000;
    address private pancakeAddress = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    // Balances and allowances
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    // Fee and exemption mappings
    mapping(address => bool) public isFeeExempt;
    mapping(address => bool) public isTxLimitExempt;
    mapping(address => bool) public isBlacklisted;

    // Fee percentages
    uint256 public LpFee = 3;
    uint256 public MwFee = 5;
    uint256 public OwnerFee = 0;
    uint256 public CharityFee = 1;

    // Total fees
    uint256 public TotalFee = 9;
    uint256 public TotalFeeSell = 9;

    // Fee options
    bool public takeBuyFee = true;
    bool public takeSellFee = true;
    bool public takeTransferFee = true;

    // Restriction options
    bool public restrictWhales = true;

    // Wallet addresses
    address private AutoLpReceiver;
    address private Mw;
    address private OwnerWallet;
    address private CharityWallet;

    // PancakeSwapRouter and pair addresses
    PancakeSwapRouter public router;
    address public pair;

    // Trading status and blacklisting options
    uint256 public launchedAt;
    bool public tradingOpen = false;
    bool public blacklistMode = true;
    bool public canUseBlacklist = true;
    
    // Swap and liquify options
    bool private inSwapAndLiquify;
    bool public swapAndLiquifyEnabled = true;
    bool public swapAndLiquifyByLimitOnly = false;
    uint256 public swapThreshold = TotalSupply * 4 / 2000;

    // Events
    event AutoLiquify(uint256 amountBNB, uint256 amountBOG);

    // Modifier for locking the swap
    modifier lockTheSwap {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }

    constructor() {
        router = PancakeSwapRouter(pancakeAddress);
        pair = PancakeSwapFactory(router.factory()).createPair(router.WETH(), address(this));
        _allowances[address(this)][address(router)] = type(uint256).max;
        _allowances[address(this)][address(pair)] = type(uint256).max;

        isFeeExempt[msg.sender] = true;
        isFeeExempt[address(this)] = true;
        isFeeExempt[DEAD_WALLET] = true;

        isTxLimitExempt[msg.sender] = true;
        isTxLimitExempt[pair] = true;
        isTxLimitExempt[DEAD_WALLET] = true;

        AutoLpReceiver = 0x876Fa48357dE62364067B84973062ff6341185cF;
        Mw = 0x8A58207c7d7b33449680056415739149e9C5EAb3;
        OwnerWallet = 0x876Fa48357dE62364067B84973062ff6341185cF;
        CharityWallet = 0x736aa6B2687DBd7Ab70f07A328de345e548E1eDe;
        
        isFeeExempt[Mw] = true;
        TotalFee = LpFee.add(MwFee).add(OwnerFee).add(CharityFee);
        TotalFeeSell = TotalFee;

        _balances[msg.sender] = TotalSupply;
        emit Transfer(address(0), msg.sender, TotalSupply);
    }

    receive() external payable {}

    function getCirculatingSupply() public view returns (uint256) {return TotalSupply.sub(balanceOf(DEAD_WALLET)).sub(balanceOf(ZERO_WALLET));}
    function allowance(address holder, address spender) external view override returns (uint256) {return _allowances[holder][spender];}
    function approveMax(address spender) external returns (bool) {return approve(spender, type(uint256).max);}
    function balanceOf(address account) public view override returns (uint256) {return _balances[account];}
    function totalSupply() external view override returns (uint256) {return TotalSupply;}
    function symbol() external pure override returns (string memory) {return Symbol;}
    function getOwner() external view override returns (address) {return _owner();}
    function name() external pure override returns (string memory) {return Name;}
    function decimals() external pure override returns (uint8) {return Decimals;}
    function launched() internal view returns (bool) {return launchedAt != 0;}
    function launch() internal {launchedAt = block.number;}

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        if (_allowances[sender][msg.sender] != type(uint256).max) {
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender].sub(amount, "Insufficient Allowance");
        }
        return _transferFrom(sender, recipient, amount);
    }

    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function _transferFrom(address sender, address recipient, uint256 amount) internal returns (bool) {
        if (inSwapAndLiquify) {return _basicTransfer(sender, recipient, amount);}
        if(!authorizations[sender] && !authorizations[recipient]){
            require(tradingOpen, "Trading not open yet");
        }

        require(amount <= TransactionMax || isTxLimitExempt[sender], "TX Limit Exceeded");
        if (msg.sender != pair && !inSwapAndLiquify && swapAndLiquifyEnabled && _balances[address(this)] >= swapThreshold) {marketingAndLiquidity();}
        if (!launched() && recipient == pair) {
            require(_balances[sender] > 0, "Zero balance violated!");
            launch();
        }    

        if (blacklistMode) {
            require(!isBlacklisted[sender],"Blacklisted");
        }

         _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");

        if (!isTxLimitExempt[recipient] && restrictWhales) {
            require(_balances[recipient].add(amount) <= WalletMax, "Max wallet violated!");
        }

        uint256 finalAmount = !isFeeExempt[sender] && !isFeeExempt[recipient] ? extractFee(sender, recipient, amount) : amount;
        _balances[recipient] = _balances[recipient].add(finalAmount);

        emit Transfer(sender, recipient, finalAmount);
        return true;
    }

    function extractFee(address sender, address recipient, uint256 amount) internal returns (uint256) {
        uint feeApplicable = 0;
        if (recipient == pair && takeSellFee) {
            feeApplicable = TotalFeeSell;        
        }
        if (sender == pair && takeBuyFee) {
            feeApplicable = TotalFee;        
        }
        if (sender != pair && recipient != pair){
            if (takeTransferFee){
                feeApplicable = TotalFeeSell; 
            }
            else{
                feeApplicable = 0;
            }
        }
        uint256 feeAmount = amount.mul(feeApplicable).div(100);

        _balances[address(this)] = _balances[address(this)].add(feeAmount);
        emit Transfer(sender, address(this), feeAmount);

        return amount.sub(feeAmount);
    }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        return _transferFrom(msg.sender, recipient, amount);
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function checkTxLimit(address sender, uint256 amount) internal view {
        require(amount <= TransactionMax || isTxLimitExempt[sender], "TX Limit Exceeded");
    }

    function marketingAndLiquidity() internal lockTheSwap {
        uint256 tokensToLiquify = _balances[address(this)];
        uint256 amountToLiquify = tokensToLiquify.mul(LpFee).div(TotalFee).div(2);
        uint256 amountToSwap = tokensToLiquify.sub(amountToLiquify);

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();

        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            amountToSwap,
            0,
            path,
            address(this),
            block.timestamp
        );

        uint256 amountBNB = address(this).balance;
        uint256 totalBNBFee = TotalFee.sub(LpFee.div(2));
        uint256 amountBNBLiquidity = amountBNB.mul(LpFee).div(totalBNBFee).div(2);
        uint256 amountBNBMarketing = amountBNB.mul(MwFee).div(totalBNBFee);
        uint256 amountBNBDev = amountBNB.mul(OwnerFee).div(totalBNBFee);
        uint256 amountBNBChar = amountBNB.mul(CharityFee).div(totalBNBFee);
        
        (bool tmpSuccess1,) = payable(Mw).call{value : amountBNBMarketing, gas : 30000}("");
        tmpSuccess1 = false;

        (tmpSuccess1,) = payable(CharityWallet).call{value : amountBNBChar, gas : 30000}("");
        tmpSuccess1 = false;

        (tmpSuccess1,) = payable(OwnerWallet).call{value : amountBNBDev, gas : 30000}("");
        tmpSuccess1 = false;

        if (amountToLiquify > 0) {
            router.addLiquidityETH{value : amountBNBLiquidity}(
                address(this), amountToLiquify, 0, 0, AutoLpReceiver, block.timestamp);
            emit AutoLiquify(amountBNBLiquidity, amountToLiquify);
        }
    }
// =======================================================================================================================
    // OWNER FUNCTIONS

    // Open trading for the contract
    function openTrading() public onlyOwner {
        tradingOpen = true;
    }

    // Set the maximum wallet balance limit
    function setWalletLimit(uint256 newLimit) external onlyOwner {
        require(newLimit >= 5, "Wallet Limit needs to be at least 0.5%");
        WalletMax = TotalSupply * newLimit / 1000;
    }

    // Set the maximum transaction amount limit
    function setTxLimit(uint256 newLimit) external onlyOwner {
        require(newLimit >= 5, "Wallet Limit needs to be at least 0.5%");
        TransactionMax = TotalSupply * newLimit / 1000;
    }

    // Set various fees for the contract
    function setFees(uint256 newLiqFee, uint256 newMwFee, uint256 newOwnerFee, uint256 newCharityFee, uint256 extraSellFee) external onlyOwner {
        LpFee = newLiqFee;
        MwFee = newMwFee;
        OwnerFee = newOwnerFee;
        CharityFee = newCharityFee;

        TotalFee = LpFee.add(MwFee).add(OwnerFee).add(CharityFee);
        TotalFeeSell = TotalFee + extraSellFee;
        require (TotalFeeSell < 25);
    }

    // Set fee receivers for various fees
    function setFeeReceivers(address newMktWallet, address newOwnerWallet, address newLpWallet, address newCharityWallet) public onlyOwner{
        AutoLpReceiver = newLpWallet;
        Mw = newMktWallet;
        OwnerWallet = newOwnerWallet;
        CharityWallet = newCharityWallet;
    }

    // Fully whitelist an address (exempt from fees and transaction limits)
    function fullWhitelist(address target) public onlyOwner{
        authorizations[target] = true;
        isFeeExempt[target] = true;
        isTxLimitExempt[target] = true;
    }

    // Set address authorization status
    function isAuth(address _address, bool status) public onlyOwner{authorizations[_address] = status;}
    // Disable the ability to add new addresses to the blacklist
    function renounceBlacklist() public onlyOwner{canUseBlacklist = false;}
    // Set the ability to charge a fee on buy transactions
    function setTakeBuyfee(bool status) public onlyOwner{takeBuyFee = status;}
    // Set the ability to charge a fee on sell transactions
    function setTakeSellfee(bool status) public onlyOwner{takeSellFee = status;}
    // Set the ability to charge a fee on transfer transactions
    function setTakeTransferfee(bool status) public onlyOwner{takeTransferFee = status;}

    // Set the trading status of the contract
    function tradingStatus(bool newStatus) public onlyOwner {
        require(canUseBlacklist, "Dev can no longer pause trading");
        tradingOpen = newStatus;
    }

    // Set address fee exemption status
    function setIsFeeExempt(address holder, bool exempt) external onlyOwner {
        isFeeExempt[holder] = exempt;
    }

    // Set address transaction limit exemption status
    function setIsTxLimitExempt(address holder, bool exempt) external onlyOwner {
        isTxLimitExempt[holder] = exempt;
    }

    // Enable or disable the use of the blacklist
    function enable_blacklist(bool _status) public onlyOwner {
        require(canUseBlacklist, "Dev can no longer add blacklists");
        blacklistMode = _status;
    }
    
    // Manage the blacklist by adding or removing addresses
    function manage_blacklist(address[] calldata addresses, bool status) public onlyOwner {
        require(canUseBlacklist, "Dev can no longer add blacklists");
        for (uint256 i; i < addresses.length; ++i) {
            isBlacklisted[addresses[i]] = status;
        }
    }

    // Set swap and liquidity settings
    function setSwapbackSettings(bool status, uint256 newAmount) public onlyOwner{
        swapAndLiquifyEnabled = status;
        swapThreshold = newAmount;
    }

    // Rescue tokens sent accidentally to the contract (CAUTION: Use with care)
    function rescueToken(address tokenAddress, uint256 tokens) public onlyOwner returns (bool success) {
        require(tokenAddress != address(this), "Cant remove the native token");
        return IBEP20(tokenAddress).transfer(msg.sender, tokens);
    }

    // Clear any stuck balance in the contract (CAUTION: Use with care)
    function clearStuckBalance(uint256 amountPercentage) external onlyOwner {
        uint256 amountETH = address(this).balance;
        payable(msg.sender).transfer(amountETH * amountPercentage / 100);
    }

    // Disable the use of the blacklist entirely (CAUTION: Should only be used when absolutely necessary)
    //function disableBlacklistDONTUSETHIS() public onlyOwner{blacklistMode = false;}
}