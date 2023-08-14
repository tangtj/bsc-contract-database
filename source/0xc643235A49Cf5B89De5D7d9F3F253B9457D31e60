// SPDX-License-Identifier: Unlicensed

pragma solidity 0.8.19;

// IERC20 interface 
interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
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

interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}

interface IUniswapV2Router02 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

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
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }
}

contract Aitechmasters is Context, IERC20, Ownable {
   using SafeMath for uint256;
    
    string private _name     = "Aitechmasters";
    string private _symbol   = "ATMCOIN";
    uint8  private _decimals = 18;
    
    IUniswapV2Router02 public _uniswapV2Router; 
    address public _uniswapV2Pair; 
    
    address payable public marketingWallet = payable(0x16319925DAC39d499cc3fAeDa26Be3f6Cd3a7109);

    mapping (address => uint256) private _rOwned;
    mapping (address => uint256) private _tOwned;
    mapping (address => mapping (address => uint256)) private _allowances;

    mapping (address => bool) private _isExcludedFromFees;
    mapping (address => bool) private _isExcluded;
    address[] private _excluded;
   
    uint256 private constant MAX = ~uint256(0);
    uint256 private _tTotal = 900000000 * 10**18;
    uint256 private _rTotal = (MAX - (MAX % _tTotal));
    uint256 private _totalReflections; // Total reflections

    uint256 public reflectionFee = 0; 
    uint256 private _previousReflectionFee = 0;

    uint256 public marketingFee = 0; 
    uint256 private _previousMarketingFee = 0;

    uint256 public  _totalTax = reflectionFee.add(marketingFee); 
    uint256 private _previousTotalTax = 0;

    uint256 private _previousTaxFee;

    //sell tax
    uint256 public reflectionSellFee = 19; 
    uint256 public marketingSellFee = 1; 
    uint256 public  _totalSellTax = reflectionSellFee.add(marketingSellFee); 

    uint256 public _maxBuyTxAmount        = 4500000 * 10**18;  
    uint256 public _maxSellTxAmount        = 4500000 * 10**18;  
    uint256 public _tokenSwapThreshold = 90000 * 10**18; 
    uint256 public _maxWalletToken = 4500000 * (10**18);

    uint256 swapPercent = 50;
        
    bool swapping;
    bool public _enableSwap = true; 

     modifier lockSwapping {
        swapping = true;
        _;
        swapping = false;
    }
    event Swap(
        uint256 tokensSwapped,
        uint256 ethReceived,
        uint256 tokensIntoLiqudity
    );
    
    
    constructor () {
        _rOwned[_msgSender()] = _rTotal;
        
        // Exclude the owner and the contract from paying fees
        _isExcludedFromFees[owner()] = true;
        _isExcludedFromFees[address(this)] = true;
        
        // Set up the uniswap V2 router
        IUniswapV2Router02 uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory())
            .createPair(address(this), uniswapV2Router.WETH());
        _uniswapV2Router = uniswapV2Router;
        
        emit Transfer(address(0), _msgSender(), _tTotal);
    }
    
    
    //recieve Eth from Uniswap V2 Router when swaping
    receive() external payable {}
     

    function name() external view returns (string memory) {
        return _name;
    }

    function symbol() external view returns (string memory) {
        return _symbol;
    }

    function decimals() external view returns (uint8) {
        return _decimals;
    }

    function totalSupply() external view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        if (_isExcluded[account]) return _tOwned[account];
        return tokenFromReflection(_rOwned[account]);
    }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) external view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) external virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) external virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function getTotalReflections() external view returns (uint256) {
        return _totalReflections;
    }
    
    function isExcludedFromFee(address account) external view returns(bool) {
        return _isExcludedFromFees[account];
    }
    
    function isExcludedFromReflection(address account) external view returns(bool) {
        return _isExcluded[account];
    }
    
    function excludeFromFee(address account) external onlyOwner() {
        _isExcludedFromFees[account] = true;
    }
    
    function includeInFee(address account) external onlyOwner() {
        _isExcludedFromFees[account] = false;
    }
    
    function reflectionFromToken(uint256 tAmount, bool deductTransferFee) public view returns(uint256) {
        require(tAmount <= _tTotal, "Amount must be less than supply");
        if (!deductTransferFee) {
            (uint256 rAmount,,,,) = _getValues(tAmount);
            return rAmount;
        } else {
            (,uint256 rTransferAmount,,,) = _getValues(tAmount);
            return rTransferAmount;
        }
    }

    function tokenFromReflection(uint256 rAmount) public view returns(uint256) {
        require(rAmount <= _rTotal, "Amount must be less than total reflections");
        uint256 currentRate =  _getRate();
        return rAmount.div(currentRate);
    }
    
    function removeAllFees() private {
        if(_totalTax == 0) return;
        
        _previousTaxFee = _totalTax;
        _totalTax = 0;
    }
    
    function restoreAllFees() private {
        _totalTax = _previousTaxFee;
    }
    
    function _getValues(uint256 tAmount) private view returns (uint256, uint256, uint256, uint256, uint256) {
        (uint256 tTransferAmount, uint256 tFee) = _getTValues(tAmount);
        (uint256 rAmount, uint256 rTransferAmount, uint256 rFee) = _getRValues(tAmount, tFee, _getRate());
        return (rAmount, rTransferAmount, rFee, tTransferAmount, tFee);
    }

    function _getTValues(uint256 tAmount) private view returns (uint256, uint256) {
        uint256 tFee = tAmount.mul(_totalTax).div(100);
        uint256 tTransferAmount = tAmount.sub(tFee);
        return (tTransferAmount, tFee);
    }

    function _getRValues(uint256 tAmount, uint256 tFee, uint256 currentRate) private pure returns (uint256, uint256, uint256) {
        uint256 rAmount = tAmount.mul(currentRate);
        uint256 rFee = tFee.mul(currentRate);
        uint256 rTransferAmount = rAmount.sub(rFee);
        return (rAmount, rTransferAmount, rFee);
    }


    function _getRate() private view returns(uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply.div(tSupply);
    }

    function _getCurrentSupply() private view returns(uint256, uint256) {
        uint256 rSupply = _rTotal;
        uint256 tSupply = _tTotal;      
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_rOwned[_excluded[i]] > rSupply || _tOwned[_excluded[i]] > tSupply) return (_rTotal, _tTotal);
            rSupply = rSupply.sub(_rOwned[_excluded[i]]);
            tSupply = tSupply.sub(_tOwned[_excluded[i]]);
        }
        if (rSupply < _rTotal.div(_tTotal)) return (_rTotal, _tTotal);
        return (rSupply, tSupply);
    }
    
    function excludeFromReward(address account) external onlyOwner() {
        require(!_isExcluded[account], "Account is already excluded");
        if(_rOwned[account] > 0) {
            _tOwned[account] = tokenFromReflection(_rOwned[account]);
        }
        _isExcluded[account] = true;
        _excluded.push(account);
    }

    function includeInReward(address account) external onlyOwner() {
        require(_isExcluded[account], "Account is already included");
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_excluded[i] == account) {
                _excluded[i] = _excluded[_excluded.length - 1];
                _tOwned[account] = 0;
                _isExcluded[account] = false;
                _excluded.pop();
                break;
            }
        }
    }
    
    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        
        // Only excluded account can bypass the max transfer amount
        if(from==_uniswapV2Pair && !_isExcludedFromFees[from] && !_isExcludedFromFees[to]) {
            require(amount <= _maxBuyTxAmount, "amount exceeds the maxBuyTxAmount.");
        }

        // Only excluded account can bypass the max transfer amount
        if(to==_uniswapV2Pair && !_isExcludedFromFees[from] && !_isExcludedFromFees[to]) {
            require(amount <= _maxSellTxAmount, "amount exceeds the maxBuyTxAmount.");
        }

        if(from==_uniswapV2Pair && !_isExcludedFromFees[from] && !_isExcludedFromFees[to]){
            uint256 contractBalanceRecepient = balanceOf(to);
            require(contractBalanceRecepient + amount <= _maxWalletToken, "Exceeds maximum wallet token amount.");
        }
        
        uint256 tokenBalance = balanceOf(address(this));
        
        // maarkting fee colllection
        if (_enableSwap && tokenBalance >= _tokenSwapThreshold && !swapping && to == _uniswapV2Pair) {
            tokenBalance = _tokenSwapThreshold;
            swapTokensForEth(tokenBalance.div(swapPercent), marketingWallet);
        }
                
        // If any account belongs to _isExcludedFromFee account then remove the fee
        bool takeFee = !(_isExcludedFromFees[from] || _isExcludedFromFees[to]);

        // Remove fees completely from the transfer if either wallet are excluded
        if (!takeFee) {
            removeAllFees();
        }
        
        // Transfer the token amount from sender to receipient.
        _tokenTransfer(from, to, amount);
        
        // If we removed the fees for this transaction, then restore them for future transactions
        if (!takeFee) {
            restoreAllFees();
        }
        
    }
    
    function _tokenTransfer(address sender, address recipient, uint256 tAmount) private {

        bool takeFee = !(_isExcludedFromFees[sender] || _isExcludedFromFees[recipient]);

        if(takeFee && recipient==_uniswapV2Pair) {enableSellFee(); }

        // Calculate the values required to execute a transfer
        (uint256 tTransferAmount, uint256 tFee) = _getTValues(tAmount);
        (uint256 rAmount, uint256 rTransferAmount,) = _getRValues(tAmount, tFee, _getRate());
        
        // Transfer from sender to recipient
		if (_isExcluded[sender]) {
		    _tOwned[sender] = _tOwned[sender].sub(tAmount);
		}
		_rOwned[sender] = _rOwned[sender].sub(rAmount);
		
		if (_isExcluded[recipient]) {
            _tOwned[recipient] = _tOwned[recipient].add(tTransferAmount);
		}
		_rOwned[recipient] = _rOwned[recipient].add(rTransferAmount); 
		
		if (tFee > 0) {
	    	uint256 swapTokens = tFee.mul(marketingFee).div(_totalTax);
            uint256 reflectionTokens = tFee.mul(reflectionFee).div(_totalTax);

            // Reflect some of the taxed tokens 
    		_reflectTokens(reflectionTokens);
            
            // Take the rest of the taxed tokens for liquidity and marketing
            if(swapTokens > 0) {
                _takeTokens(swapTokens);
                emit Transfer(sender, address(this), swapTokens);
            }
		}
            
        emit Transfer(sender, recipient, tTransferAmount);

        if(takeFee && recipient==_uniswapV2Pair) {restoreBuyFee(); }
    }

    function _reflectTokens(uint256 tFee) private {
        uint256 rFee = tFee.mul(_getRate());
        _rTotal = _rTotal.sub(rFee);
        _totalReflections = _totalReflections.add(tFee);
    }
    
    function _takeTokens(uint256 tTakeAmount) private {
        uint256 currentRate = _getRate();
        uint256 rTakeAmount = tTakeAmount.mul(currentRate);
        _rOwned[address(this)] = _rOwned[address(this)].add(rTakeAmount);
        if(_isExcluded[address(this)]) {
            _tOwned[address(this)] = _tOwned[address(this)].add(tTakeAmount);
        }
    }
    
    function swapTokensForEth(uint256 tokenAmount, address _to) private lockSwapping {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _uniswapV2Router.WETH(); 

        _approve(address(this), address(_uniswapV2Router), tokenAmount);

        // Execute the swap
        _uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // Accept any amount of Eth
            path,
            _to,
            block.timestamp.add(300)
        );
    }
    
    function reflect(uint256 tAmount) public {
        require(!_isExcluded[_msgSender()], "Excluded addresses cannot call this function");
        (uint256 rAmount,,,,) = _getValues(tAmount);
        _rOwned[_msgSender()] = _rOwned[_msgSender()].sub(rAmount);
        _rTotal = _rTotal.sub(rAmount);
        _totalReflections = _totalReflections.add(tAmount);
    }

    function enableSellFee() internal {
        _previousReflectionFee = reflectionFee;
        _previousMarketingFee = marketingFee;
    
        _previousTotalTax = _totalTax;

        reflectionFee = reflectionSellFee;
        marketingFee = marketingSellFee;
        _totalTax = reflectionSellFee.add(marketingSellFee);
    }

    function restoreBuyFee() internal {
        reflectionFee = _previousReflectionFee;
        marketingFee = _previousMarketingFee;
        _totalTax = _previousReflectionFee.add(_previousMarketingFee);
    }

    function setBuyFee(uint256 reflection, uint256 marketing) external onlyOwner() {
        reflectionFee = reflection;
        marketingFee = marketing;
        _totalTax = reflectionFee.add(marketingFee);
        require(_totalTax <= 10, "tax too high");
    }

    function setSellFee(uint256 reflection, uint256 marketing) external onlyOwner() {
        reflectionSellFee = reflection;
        marketingSellFee = marketing;
        _totalSellTax = reflectionSellFee.add(marketingSellFee);
        require(_totalSellTax <= 20, "tax too high");
    }

    function setMaxWalletLimit(uint256 _newLimit) public onlyOwner {
        _maxWalletToken = _newLimit;
        require(_maxWalletToken >= _tTotal.div(500), "value too low");
    }

    function setMaxBuyTxAmount(uint256 maxTxAmount) external onlyOwner() {
        _maxBuyTxAmount = maxTxAmount;
        require(_maxBuyTxAmount >= _tTotal.div(500), "value too low");
    }

    function setMaxSellTxAmount(uint256 maxTxAmount) external onlyOwner() {
        _maxSellTxAmount = maxTxAmount;
        require(_maxSellTxAmount >= _tTotal.div(500), "value too low");
    }
    
    function setTokenSwapThreshold(uint256 tokenSwapThreshold) external onlyOwner() {
        _tokenSwapThreshold = tokenSwapThreshold;
    }
    
    function updateWallet(address _marketingWallet) external onlyOwner() {
        marketingWallet = payable(_marketingWallet);
    }

    function setSwapPercent(uint256 _swapPercent) public onlyOwner {
        swapPercent = _swapPercent;
    }

    function setSwapAndEnabled(bool _enable) external onlyOwner() {
        _enableSwap = _enable;
    }

}