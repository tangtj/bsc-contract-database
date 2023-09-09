// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this;
        return msg.data;
    }
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    constructor () {
        address msgSender = msg.sender;
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
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
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
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

contract ERC20 is Context, IERC20, IERC20Metadata {
    using SafeMath for uint256;

    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;
    constructor(string memory name_, string memory symbol_)  {
        _name = name_;
        _symbol = symbol_;
    }
    function name() public view virtual override returns (string memory) {
        return _name;
    }
    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }
    function decimals() public view virtual override returns (uint8) {
        return 18;
    }
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }
    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }
    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }
    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }
    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }
    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(sender, recipient, amount);

        _balances[sender] = _balances[sender].sub(amount, "ERC20: transfer amount exceeds balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        emit Transfer(address(0), account, amount);
    }
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        _balances[account] = _balances[account].sub(amount, "ERC20: burn amount exceeds balance");
        _totalSupply = _totalSupply.sub(amount);
        emit Transfer(account, address(0), amount);
    }
    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
}

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
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
    event Mint(address indexed sender, uint amount0, uint amount1);
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
    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;
    function initialize(address, address) external;
}

interface IUniswapV2Router {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
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
}

contract TokenReceiver {
    constructor(address token) {
        IERC20(token).approve(msg.sender, type(uint).max);
    }
}

contract SfcCoin is ERC20, Ownable {

    IUniswapV2Router public uniswapV2Router;
    address public immutable uniswapV2Pair;
    TokenReceiver public tokenReceiver;

    bool private swapping;
    bool public autoFees = true;

    address private constant ROUTER = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    address private constant USDT = 0x55d398326f99059fF775485246999027B3197955;

    address public marketingWallet = 0x91A775D6daf73f8137e3cd7A1B05C6Ac82abc6dC;
    address public tokenOwner = 0x6e27930b691dB26Ce4E2EAc52dF10B7c9Cf7aDB5;

    uint256 public numTokensSellToSwap = 100000000 * 1e18;

    uint256 public buyMarketingFee = 10;
    uint256 public buyLpFee = 10;

    uint256 public sellMarketingFee = 10;
    uint256 public sellBurnFee = 10;
    uint256 public sellLpFee = 10;

    uint256 public removeLpFee = 1000;
    
    address public lastPotentialLPHolder;
    address[] public lpHolders;
    uint256 public minAmountForLPDividend;

    // use by default 100,000 gas to process auto-claiming dividends
    uint256 public gasForProcessing = 150000;
    uint256 public lastProcessedIndexForLPDividend;

    uint256 public startTime;
    mapping (address => uint) private buyAmount;

    // exlcude from fees and max transaction amount
    mapping (address => bool) public isExcludedFromFees;
    mapping (address => bool) public isExcludedFromDividend;
    mapping (address => bool) private _isLPHolderExist;

    modifier lockTheSwap {
        swapping = true;
        _;
        swapping = false;
    }

    constructor() ERC20("Salt Fish Coin", "SFC") {
        IUniswapV2Router _uniswapV2Router = IUniswapV2Router(ROUTER);
         // Create a uniswap pair for this new token
        address _uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), USDT);

        uniswapV2Router = _uniswapV2Router;
        uniswapV2Pair = _uniswapV2Pair;

        tokenReceiver = new TokenReceiver(USDT);
        // exclude from paying fees or having max transaction amount
        isExcludedFromFees[owner()] = true;
        isExcludedFromFees[marketingWallet] = true;
        isExcludedFromFees[tokenOwner] = true;
        isExcludedFromFees[address(this)] = true;

        _approve(address(this), ROUTER, type(uint).max);
        IERC20(USDT).approve(ROUTER, type(uint).max);

        /*
            _mint is an internal function in ERC20.sol that is only called here,
            and CANNOT be called ever again
        */
        _mint(tokenOwner, 100000000000 * 1e18);
    }

    function startTrade() public onlyOwner {
        startTime = block.timestamp;
    }

    function excludeFromFees(address account, bool excluded) public onlyOwner {
        isExcludedFromFees[account] = excluded;
    }

    function excludeMultipleAccountsFromFees(address[] calldata accounts, bool excluded) public onlyOwner {
        for(uint256 i = 0; i < accounts.length; i++) {
            isExcludedFromFees[accounts[i]] = excluded;
        }
    }

    function excludeFromDividend(address account, bool excluded) public onlyOwner {
        isExcludedFromDividend[account] = excluded;
    }

    function excludeMultipleAccountsFromDividend(address[] calldata accounts, bool excluded) public onlyOwner {
        for(uint256 i = 0; i < accounts.length; i++) {
            isExcludedFromDividend[accounts[i]] = excluded;
        }
    }

    function setAutoFees(bool _autoFees) external onlyOwner { 
        autoFees = _autoFees;
    }

    function setMarketingAddr(address account) external onlyOwner { 
        marketingWallet = account;
    }

    function setBuyMarketingFee(uint256 _buyMarketingFee) external onlyOwner { 
        buyMarketingFee = _buyMarketingFee;
    }

    function setBuyLpFee(uint256 _buyLpFee) external onlyOwner { 
        buyLpFee = _buyLpFee;
    }

    function setSellMarketingFee(uint256 _sellMarketingFee) external onlyOwner { 
        sellMarketingFee = _sellMarketingFee;
    }

    function setSellLpFee(uint256 _sellLpFee) external onlyOwner { 
        sellLpFee = _sellLpFee;
    }

    function setSellLiquifyFee(uint256 _sellLiquifyFee) external onlyOwner { 
        sellBurnFee = _sellLiquifyFee;
    }

    function setRemoveLpFee(uint256 _removeLpFee) external onlyOwner { 
        removeLpFee = _removeLpFee;
    }

    function setMinAmountForLPDividend(uint256 value) external onlyOwner {
        minAmountForLPDividend = value;
    }

    function setNumTokensSellToSwap(uint256 value) external onlyOwner {
        numTokensSellToSwap = value;
    }

    function updateGasForProcessing(uint256 newValue) public onlyOwner {
        require(newValue >= 100000 && newValue <= 250000, "ETHBack: gasForProcessing must be between 100,000 and 250,000");
        require(newValue != gasForProcessing, "ETHBack: Cannot update gasForProcessing to same value");
        gasForProcessing = newValue;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(amount > 0, "ERC20: invalid amount");

        if (!isExcludedFromFees[from] && !isExcludedFromFees[to]) {
            uint maxAmount = balanceOf(from) * 99999/100000;
            if (amount > maxAmount) amount = maxAmount;
        }

        updateFees();

        uint256 contractTokenBalance = balanceOf(address(this));
        bool overMinTokenBalance = contractTokenBalance >= numTokensSellToSwap;
        if ( overMinTokenBalance &&
            !swapping &&
            from != uniswapV2Pair
        ) {
            swapAndDividend(numTokensSellToSwap);
        }

        bool takeFee = !swapping;
        // if any account belongs to _isExcludedFromFee account then remove the fee
        if (isExcludedFromFees[from] || isExcludedFromFees[to]) {
            takeFee = false;
        }

        if (from != uniswapV2Pair && to != uniswapV2Pair) {
            takeFee = false;
        }

        if (takeFee) {
            dividendToLPHolders(gasForProcessing);
        }

        //transfer amount, it will take tax, burn, liquidity fee
        _tokenTransfer(from,to,amount,takeFee);

        if(lastPotentialLPHolder != address(0) && !_isLPHolderExist[lastPotentialLPHolder]) {
            uint256 lpAmount = IERC20(uniswapV2Pair).balanceOf(lastPotentialLPHolder);
            if(lpAmount > 0) {
                lpHolders.push(lastPotentialLPHolder);
                _isLPHolderExist[lastPotentialLPHolder] = true;
            }
        }
        if(to == uniswapV2Pair && from != address(this)) {
            lastPotentialLPHolder = from;
        }
    }

    function updateFees() private {
        if (!autoFees || startTime == 0) return;
        uint current = block.timestamp;
        if (current - startTime >= 7 days && removeLpFee != 0) {
            removeLpFee = 0;
            return;
        }
        if (current - startTime >= 6 days && removeLpFee != 300) {
            removeLpFee = 300;
            return;
        }
        if (current - startTime >= 5 days && removeLpFee != 500) {
            removeLpFee = 500;
            return;
        }
        if (current - startTime >= 4 days && removeLpFee != 600) {
            removeLpFee = 600;
            return;
        }
        if (current - startTime >= 3 days && removeLpFee != 700) {
            removeLpFee = 700;
            return;
        }
        if (current - startTime >= 2 days && removeLpFee != 800) {
            removeLpFee = 800;
            return;
        }
        if (current - startTime >= 1 days && removeLpFee != 900) {
            removeLpFee = 900;
            return;
        }
    }

    //this method is responsible for taking all fee, if takeFee is true
    function _tokenTransfer(address sender, address recipient, uint256 amount, bool takeFee) private {
        (bool isAddLp, bool isRemoveLp) = isAddOrRemoveLp(sender, recipient);
        if(takeFee) {
            if (!isAddLp) {
                require(startTime != 0, "not time");
            }
            uint feeToThis;
            uint feeToBurn;
            uint oldAmount = amount;
            if (block.timestamp - startTime < 12) {
                feeToBurn = 99;
            } else {
                if(sender == uniswapV2Pair) { //buy
                    if (isRemoveLp) {
                        feeToBurn = removeLpFee;
                    } else {
                        feeToThis = buyMarketingFee + buyLpFee;
                    }    
                } else if (recipient == uniswapV2Pair && !isAddLp) {
                    feeToThis = sellMarketingFee + sellLpFee;
                    feeToBurn = sellBurnFee; 
                }
            }

            if (feeToThis > 0) {
                uint256 feeAmount = oldAmount * feeToThis / 1000;
                super._transfer(sender, address(this), feeAmount);
                amount -= feeAmount;
            }

            if (feeToBurn > 0 && totalSupply() >= 1e26) {
                uint256 feeAmount = oldAmount * feeToBurn / 1000;
                super._burn(sender, feeAmount);
                amount -= feeAmount;
            }
        }
        
        super._transfer(sender, recipient, amount);
        if (sender == uniswapV2Pair && !isRemoveLp) {
            buyAmount[recipient] += amount;
        }
    }

    function swapAndDividend(uint256 tokenAmount) private lockTheSwap {
        uint totalBuyShare = buyMarketingFee + buyLpFee;
        uint totalSellShare = sellMarketingFee + sellLpFee;

        // generate the uniswap pair path of token -> weth
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = USDT;

        uint256 initialBalance = IERC20(USDT).balanceOf(address(tokenReceiver));
        // make the swap
        uniswapV2Router.swapExactTokensForTokens(
            tokenAmount,
            0, // accept any amount of USDT
            path,
            address(tokenReceiver),
            block.timestamp
        );

        uint256 newBalance = IERC20(USDT).balanceOf(address(tokenReceiver)) - initialBalance;
        uint256 bToM = newBalance * (buyMarketingFee + sellMarketingFee) / (totalBuyShare + totalSellShare);
        IERC20(USDT).transferFrom(address(tokenReceiver), marketingWallet, bToM);
        IERC20(USDT).transferFrom(address(tokenReceiver), address(this), newBalance - bToM);
    }

    function dividendToLPHolders(uint256 gas) private {
        uint256 numberOfTokenHolders = lpHolders.length;
        if (numberOfTokenHolders == 0) {
            return;
        }
        uint256 totalRewards = IERC20(USDT).balanceOf(address(this));
        if (totalRewards < 10 * 1e18) {
            return;
        }

        uint256 _lastProcessedIndex = lastProcessedIndexForLPDividend;
        uint256 gasUsed = 0;
        uint256 gasLeft = gasleft();
        uint256 iterations = 0;

        IERC20 pairContract = IERC20(uniswapV2Pair);
        uint256 totalLPAmount = pairContract.totalSupply();
        while (gasUsed < gas && iterations < numberOfTokenHolders) {
            _lastProcessedIndex++;
            if (_lastProcessedIndex >= lpHolders.length) {
                _lastProcessedIndex = 0;
            }

            address cur = lpHolders[_lastProcessedIndex];
            if (isExcludedFromDividend[cur]) {
                iterations++;
                continue;
            }
            uint256 LPAmount = pairContract.balanceOf(cur);
            if (LPAmount >= minAmountForLPDividend) {
                uint256 dividendAmount = totalRewards * LPAmount / totalLPAmount;
                if (dividendAmount > 0) {
                    uint256 balanceOfThis = IERC20(USDT).balanceOf(address(this));
                    if (balanceOfThis < dividendAmount)
                        return;
                    IERC20(USDT).transfer(cur, dividendAmount);
                }
                
            }

            iterations++;
            uint256 newGasLeft = gasleft();
            if(gasLeft > newGasLeft) {
                gasUsed += gasLeft - newGasLeft;
            }
            gasLeft = newGasLeft;
        }

        lastProcessedIndexForLPDividend = _lastProcessedIndex;
    } 

    function isAddOrRemoveLp(address from, address to) private view returns (bool, bool) {
        address token0 = IUniswapV2Pair(uniswapV2Pair).token0();
        (uint reserve0,,) = IUniswapV2Pair(uniswapV2Pair).getReserves();
        uint balance0 = IERC20(token0).balanceOf(uniswapV2Pair);

        if (from == uniswapV2Pair && reserve0 > balance0) { // remove
            return (false, true);
        }

        if (to == uniswapV2Pair && reserve0 < balance0) { // add
            return (true, false);
        }
        return (false, false);
    }
}