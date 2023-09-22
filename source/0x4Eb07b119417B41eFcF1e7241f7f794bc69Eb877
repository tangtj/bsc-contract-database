// SPDX-License-Identifier: MIT
pragma solidity >=0.6.0 <0.9.0;

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
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }
}


interface IERC20 {
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

interface IFactoryV2 {
    event PairCreated(address indexed token0, address indexed token1, address lpPair, uint);
    function getPair(address tokenA, address tokenB) external view returns (address lpPair);
    function createPair(address tokenA, address tokenB) external returns (address lpPair);
}

interface IV2Pair {
    function factory() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function sync() external;
}

interface IRouter01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
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
    function swapExactETHForTokens(
        uint amountOutMin, 
        address[] calldata path, 
        address to, uint deadline
    ) external payable returns (uint[] memory amounts);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IRouter02 is IRouter01 {
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
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
}

/**
interface IPinkAntiBot {
  function setTokenOwner(address owner) external;

  function onPreTransferCheck(
    address from,
    address to,
    uint256 amount
  ) external;
}
*/

contract DonaldTrump2024Token is IERC20 {
    using SafeMath for uint256;

    mapping(address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;

    mapping(address => bool) private _isExcludedFromFee;
    mapping (address => bool) private _liquidityHolders;
   
    string constant private _name = "Donald Trump";
    string constant private _symbol = "TRUMP2024";
    uint8 constant private _decimals = 6;

    uint256 constant private _totalSupply = 100_000_000_000_000_000 * 10**_decimals;

    uint256 public taxFeeOnBuy = 10;
    uint256 public taxFeeOnSell = 10;
    uint256 public numTokensSellToSwap = 52_000_000_000_000 * 10**_decimals;

    IRouter02 public dexRouter;
	// IPinkAntiBot public pinkAntiBot;

    address public lpPair;
    address constant public DEAD = 0x000000000000000000000000000000000000dEaD;

    address private _owner;
    address payable private devAddress;

    bool public tradingEnabled = false;
    bool public hasLiqBeenAdded = false;
    bool private allowedPresaleExclusion = true;
    //bool public enableAntiBot;
    
    bool private inSwap = false;
    bool private swapEnabled;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    modifier onlyOwner() {
        require(_owner == msg.sender, "Caller =/= owner.");
        _;
    }

    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor () payable {
        // Set the owner.
        _owner = msg.sender;
        
        _balances[_owner] = _totalSupply;
        emit Transfer(address(0), _owner, _totalSupply);

        dexRouter = IRouter02(0x10ED43C718714eb63d5aA57B78B54704E256024E);

        lpPair = IFactoryV2(dexRouter.factory()).createPair(dexRouter.WETH(), address(this));
        devAddress = payable(_owner);
        _liquidityHolders[_owner] = true;

        _isExcludedFromFee[_owner] = true;
        _isExcludedFromFee[devAddress] = true;
        _isExcludedFromFee[address(this)] = true;

        _approve(_owner, address(dexRouter), type(uint256).max);
        _approve(address(this), address(dexRouter), type(uint256).max);
    }

    receive() external payable {}

    function totalSupply() external pure override returns (uint256) {
        return _totalSupply;
    }

    function decimals() external pure override returns (uint8) { 
        return _decimals;
    }

    function symbol() external pure override returns (string memory) { return _symbol; }

    function name() external pure override returns (string memory) { return _name; }

    function getOwner() external view override returns (address) { return _owner; }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function _approve(address sender, address spender, uint256 amount) internal {
        require(sender != address(0), "ERC20: Zero Address");
        require(spender != address(0), "ERC20: Zero Address");

        _allowances[sender][spender] = amount;
        emit Approval(sender, spender, amount);
    }

    function approveContractContingency() external onlyOwner returns (bool) {
        _approve(address(this), address(dexRouter), type(uint256).max);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        if (_allowances[sender][msg.sender] != type(uint256).max) {
            _allowances[sender][msg.sender] -= amount;
        }

        return _transfer(sender, recipient, amount);
    }

    function _hasLimits(address from, address to) internal view returns (bool) {
        return from != _owner
            && to != _owner
            && tx.origin != _owner
            && !_liquidityHolders[to]
            && !_liquidityHolders[from]
            && to != DEAD
            && to != address(0)
            && from != address(this);
    }

    function _transfer(address from, address to, uint256 amount) internal returns (bool) {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");

        if (_hasLimits(from, to)) {
            if(!tradingEnabled) {
                revert("Trading not yet enabled!");
            }
        }

        return finalizeTransfer(from, to, amount);
    }

    function finalizeTransfer(address from, address to, uint256 amount) internal returns (bool) {
        uint256 _taxFee = 0;
        bool other = false;

        if (from == lpPair && to != address(dexRouter)) {
            _taxFee = taxFeeOnBuy;
        } else if (to == lpPair && from != address(dexRouter)) {
            _taxFee = taxFeeOnSell;
        } else {
            other = true;
        }

        if ((_isExcludedFromFee[from] || _isExcludedFromFee[to]) || (from != lpPair && to != lpPair)) {
            _taxFee = 0;
        }

        if (!hasLiqBeenAdded) {
            _checkLiquidityAdd(from, to);
            if (!hasLiqBeenAdded && _hasLimits(from, to) && !other) {
                revert("Pre-liquidity transfer protection.");
            }
        }

        uint256 contractTokenBalance = balanceOf(address(this));

        bool overMinTokenBalance = contractTokenBalance >=
            numTokensSellToSwap;
        if (
            overMinTokenBalance &&
            !inSwap &&
            from != lpPair &&
            swapEnabled
        ) {
            contractTokenBalance = numTokensSellToSwap;
            //add liquidity
            swapTokensForEth(contractTokenBalance);

            uint256 contractETHBalance = address(this).balance;
            if (contractETHBalance > 0) {
                sendETHToFee(address(this).balance);
            }
        }

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");

        uint256 fee = amount.mul(_taxFee).div(1000);
        if(fee > 0) {
            _transferAmount(from, address(this), fee);
        }

        uint256 transferAmount = amount.sub(fee);
        _transferAmount(from, to, transferAmount);

        return true;
    }

    function _transferAmount(address _from, address _to, uint256 _amount) internal {
        _balances[_from] = _balances[_from].sub(_amount);
        _balances[_to] = _balances[_to].add(_amount);

        emit Transfer(_from, _to, _amount);
    }

    function _checkLiquidityAdd(address from, address to) internal {
        require(!hasLiqBeenAdded, "Liquidity already added and marked.");
        if (!_hasLimits(from, to) && to == lpPair) {
            _liquidityHolders[from] = true;
            hasLiqBeenAdded = true;
        }
    }

    function swapTokensForEth(uint256 tokenAmount) private lockTheSwap {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = dexRouter.WETH();
        _approve(address(this), address(dexRouter), tokenAmount);
        dexRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function sendETHToFee(uint256 amount) private {
        devAddress.transfer(amount);
    }
    
    function manualswap() external {
        require(
            msg.sender == devAddress || msg.sender == _owner
        );
        uint256 contractBalance = balanceOf(address(this));
        swapTokensForEth(contractBalance);
    }

    function manualsend() external {
        require(
            msg.sender == devAddress || msg.sender == _owner
        );
        uint256 contractETHBalance = address(this).balance;
        sendETHToFee(contractETHBalance);
    }

    function transferOwner(address _newOwner) external onlyOwner {
        require(_newOwner != address(0), "Call renounceOwnership to transfer owner to the zero address.");
        require(_newOwner != DEAD, "Call renounceOwnership to transfer owner to the zero address.");
        if (balanceOf(_owner) > 0) {
            finalizeTransfer(_owner, _newOwner, balanceOf(_owner));
        }
        
        address oldOwner = _owner;
        _owner = _newOwner;
        _isExcludedFromFee[_owner] = true;

        emit OwnershipTransferred(oldOwner, _newOwner);
    }

    function renounceOwnership() external onlyOwner {
        address oldOwner = _owner;
        _owner = address(0);
        emit OwnershipTransferred(oldOwner, address(0));
    }

    function excludePresaleAddresses(address _router, address _presale) external onlyOwner {
        require(allowedPresaleExclusion);
        require(_router != address(this) 
                && _presale != address(this) 
                && lpPair != _router 
                && lpPair != _presale, "Just don't.");
        if (_router == _presale) {
            _liquidityHolders[_presale] = true;
        } else {
            _liquidityHolders[_router] = true;
            _liquidityHolders[_presale] = true;
        }
    }

    function setDevAddress(address _newAddress) external onlyOwner {
        require(devAddress != address(0), "address cannot be 0");
        devAddress = payable(_newAddress);
    }

    function excludeMultipleAccountsFromFees(
        address[] calldata _accounts,
        bool _excluded
    ) public onlyOwner {
        for (uint256 i = 0; i < _accounts.length; i++) {
            _isExcludedFromFee[_accounts[i]] = _excluded;
        }
    }

    function enableTrading() public onlyOwner {
        require(!tradingEnabled, "Trading already enabled!");
        require(hasLiqBeenAdded, "Liquidity must be added.");
        tradingEnabled = true;
        swapEnabled = true;
        allowedPresaleExclusion = false;
    }

    function setFee(uint256 _taxFeeOnBuy, uint256 _taxFeeOnSell) public onlyOwner {
        require(_taxFeeOnBuy <= 10, "Tax cannot be more than 1.");
        require(_taxFeeOnSell <= 10, "Tax cannot be more than 1.");
        taxFeeOnBuy = _taxFeeOnBuy;
        taxFeeOnSell = _taxFeeOnSell;
    }

    function updateSwapEnabled(bool _enabled) external onlyOwner {
        swapEnabled = _enabled;
    }
}