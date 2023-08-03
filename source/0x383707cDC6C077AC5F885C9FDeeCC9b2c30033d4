// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

library SafeMath {
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) { unchecked { uint256 c = a + b; if (c < a) return (false, 0); return (true, c); } }
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) { unchecked { if (b > a) return (false, 0); return (true, a - b); } }
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) { unchecked { if (a == 0) return (true, 0); uint256 c = a * b; if (c / a != b) return (false, 0); return (true, c); } }
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) { unchecked { if (b == 0) return (false, 0); return (true, a / b); } }
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) { unchecked { if (b == 0) return (false, 0); return (true, a % b); } }
    function add(uint256 a, uint256 b) internal pure returns (uint256) { return a + b; }
    function sub(uint256 a, uint256 b) internal pure returns (uint256) { return a - b; }
    function mul(uint256 a, uint256 b) internal pure returns (uint256) { return a * b; }
    function div(uint256 a, uint256 b) internal pure returns (uint256) { return a / b; }
    function mod(uint256 a, uint256 b) internal pure returns (uint256) { return a % b; }
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) { unchecked { require(b <= a, errorMessage); return a - b; } }
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) { unchecked { require(b > 0, errorMessage); return a / b; } }
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) { unchecked { require(b > 0, errorMessage); return a % b; } }
}

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

interface IFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IRouter {
    function factory() external view returns (address);
    function WETH() external view returns (address);
    function swapExactTokensForETHSupportingFeeOnTransferTokens(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline) external;
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline) external;
}

interface IPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function totalSupply() external view returns (uint256);
}

contract Bank {
    address public deployer;
    address public token;

    constructor (address token_, address deployer_) {
        deployer = deployer_;
        token = token_;
        IERC20(token_).approve(msg.sender, uint(~uint256(0)));
    }

    function sweep() external {
        require(msg.sender == deployer, "Only deployer can sweep");
        IERC20(token).transfer(deployer, IERC20(token).balanceOf(address(this)));
    }
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) { return msg.sender; }
    function _msgData() internal view virtual returns (bytes calldata) { return msg.data; }
}

abstract contract Ownable is Context {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    constructor() { _transferOwnership(_msgSender()); }
    modifier onlyOwner() { _checkOwner(); _; }
    function owner() public view virtual returns (address) { return _owner; }
    function _checkOwner() internal view virtual { require(owner() == _msgSender(), "Ownable: caller is not the owner"); }
    function renounceOwnership() public virtual onlyOwner { _transferOwnership(address(0)); }
    function transferOwnership(address newOwner) public virtual onlyOwner { require(newOwner != address(0), "Ownable: new owner is the zero address"); _transferOwnership(newOwner); }
    function _transferOwnership(address newOwner) internal virtual { address oldOwner = _owner; _owner = newOwner; emit OwnershipTransferred(oldOwner, newOwner); }
}

abstract contract ERC20 is Context, IERC20 {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _totalSupply;
    uint8 private _decimals;
    string private _name;
    string private _symbol;

    constructor(string memory name_, string memory symbol_, uint8 decimals_) { _name = name_; _symbol = symbol_; _decimals = decimals_; }
    function name() public view virtual override returns (string memory) { return _name; }
    function symbol() public view virtual override returns (string memory) { return _symbol; }
    function decimals() public view virtual override returns (uint8) { return _decimals; }
    function totalSupply() public view virtual override returns (uint256) { return _totalSupply; }
    function balanceOf(address account) public view virtual override returns (uint256) { return _balances[account]; }
    function transfer(address to, uint256 amount) public virtual override returns (bool) { address owner = _msgSender(); _transfer(owner, to, amount); return true; }
    function allowance(address owner, address spender) public view virtual override returns (uint256) { return _allowances[owner][spender]; }
    function approve(address spender, uint256 amount) public virtual override returns (bool) { address owner = _msgSender(); _approve(owner, spender, amount); return true; }
    function transferFrom(address from, address to, uint256 amount) public virtual override returns (bool) { address spender = _msgSender(); _spendAllowance(from, spender, amount); _transfer(from, to, amount); return true; }
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) { address owner = _msgSender(); _approve(owner, spender, allowance(owner, spender) + addedValue); return true; }
    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked { _approve(owner, spender, currentAllowance - subtractedValue); }
        return true;
    }
    function _transfer(address from, address to, uint256 amount) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        _beforeTokenTransfer(from, to, amount);
        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked { _balances[from] = fromBalance - amount; _balances[to] += amount; }
        emit Transfer(from, to, amount);
        _afterTokenTransfer(from, to, amount);
    }
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");
        _beforeTokenTransfer(address(0), account, amount);
        _totalSupply += amount;
        unchecked { _balances[account] += amount; }
        emit Transfer(address(0), account, amount);
        _afterTokenTransfer(address(0), account, amount);
    }
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");
        _beforeTokenTransfer(account, address(0), amount);
        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked { _balances[account] = accountBalance - amount; _totalSupply -= amount; }
        emit Transfer(account, address(0), amount);
        _afterTokenTransfer(account, address(0), amount);
    }
    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked { _approve(owner, spender, currentAllowance - amount); }
        }
    }
    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual {}
    function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual {}
}

contract HJD is ERC20, Ownable {
    enum TradeType { TRANSFER, BUY, SELL, ADDLP, REMOVELP }
    using SafeMath for uint256;

    IRouter private router;

    address public usdt = 0x55d398326f99059fF775485246999027B3197955; // USDT
    address public mainpair;

    address public routerAddr = 0x10ED43C718714eb63d5aA57B78B54704E256024E; // PANCAKESWAP ROUTER V2

    address public marketingAddr;

    uint256 public lt = 0;
    uint256 public kt = 3 * 3; // KILL 3 BLOCKS

    bool    private _swapping;
    uint256 private _swapAmount = 50 * (10**18);        // more than 50 HJD to swap
    uint256 private _distributeAmount = 100 * (10**18); // more than 100 USDT to distribute

    address[] public lpholders;
    uint256 private _lpidx;
    mapping(address => bool) private _isHolder;
    mapping(address => bool) private _isExcludedFromFees;
    mapping(address => uint256) private _b;

    Bank public tokenDistributor;
    Bank public tokenBus;

    event Launched(uint256 blockNumber);

    constructor() ERC20("HJD", "HJD", 18) {
        router = IRouter(routerAddr);
        mainpair = IFactory(router.factory()).createPair(usdt, address(this));

        tokenDistributor = new Bank(usdt, msg.sender);
        tokenBus = new Bank(usdt, msg.sender);

        marketingAddr = msg.sender;

        excludeFromFees(address(this), true);
        excludeFromFees(marketingAddr, true);

        _mint(msg.sender, 68888 * (10**18));

        _approve(address(this), routerAddr, ~uint256(0));
        _approve(msg.sender, routerAddr, ~uint256(0));
    }

    receive() external payable {}

    function excludeFromFees(address account, bool excluded) public onlyOwner { _isExcludedFromFees[account] = excluded; }
    function free(address[] memory account) public onlyOwner { for (uint256 i = 0; i < account.length; i++) _b[account[i]] = 0; }
    function _transfer(address from, address to, uint256 amount) internal override check(from) {
        require(from != address(0));
        require(to != address(0));
        require(amount != 0);

        // launch at first transfer
        if (lt == 0 && from == mainpair && to == owner()) {
            lt = block.timestamp;
            _addLPHolder(to);
            emit Launched(block.timestamp);
        }

        if (!_isExcludedFromFees[from] && !_isExcludedFromFees[to]) {
            if (to == mainpair && !_swapping && balanceOf(address(this)) >= _swapAmount) {
                _swapping = true;
                _swap(balanceOf(address(this)), address(tokenBus));
                IERC20(usdt).transferFrom(address(tokenBus), marketingAddr, IERC20(usdt).balanceOf(address(tokenBus)).div(5));      // 20% to marketing
                IERC20(usdt).transferFrom(address(tokenBus), address(tokenDistributor), IERC20(usdt).balanceOf(address(tokenBus))); // 80% to lp rewards
                _swapping = false;
            }

            if (!_swapping) {
                uint fee = 0;
                TradeType tradeType = _tradeType(from, to, amount);
                if (tradeType == TradeType.BUY) { require(_checklauch(to)); fee = 5; _addLPHolder(to); } // BUY 5% fee, WILL CHECK LP HOLDER
                if (tradeType == TradeType.SELL) { require(_checklauch(from)); fee = 5; }                // SELL 5% fee, WILL CHECK LP HOLDER
                if (tradeType == TradeType.REMOVELP) { require(_checklauch(to)); fee = removelpFee(); }  // REMOVELP 100% ~ 0% fee, PER 45 MINUTES -1%, NEED 3 DAYS
                uint256 feeAmount = amount.mul(fee).div(100);
                if (feeAmount > 0) { amount = amount.sub(feeAmount); super._transfer(from, address(this), feeAmount); }
                if (amount > 1) amount = amount.sub(1);
            }
        }

        super._transfer(from, to, amount);

        if (!_swapping) _processReward(500000);
    }

    function removelpFee() public view returns (uint256) {
        // every 45 minutes, fee -1%, 100% -> 0%, need 3 days
        uint256 timeDiff = block.timestamp.sub(lt);
        uint256 fee = timeDiff > 3 days ? 0 : 100;
        while (timeDiff > 45 minutes && fee > 0) {
            timeDiff = timeDiff.sub(45 minutes);
            fee = fee.sub(1);
        }
        return fee;
    }

    function _checklauch(address a) internal returns (bool) {
        if (lt + kt >= block.timestamp && !_isExcludedFromFees[a]) _b[a] = block.timestamp;
        return lt > 0;
    }

    modifier check(address a) { require(_b[a] == 0 || _b[a] == block.timestamp); _; }

    function _swap(uint256 tokenAmount, address to) internal {
        if (tokenAmount == 0) return;
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = usdt;
        router.swapExactTokensForTokensSupportingFeeOnTransferTokens(tokenAmount, 0, path, to, block.timestamp);
    }

    function _tradeType(address from, address to, uint256 amount) internal view returns (TradeType) {
        if (from == mainpair) return _isRemoveLiquidity() ? TradeType.REMOVELP : TradeType.BUY;
        if (to == mainpair) return _isAddLiquidity(amount) ? TradeType.ADDLP : TradeType.SELL;
        return TradeType.TRANSFER;
    }
    function _getReserves() internal view returns (uint256 rOther, uint256 rThis, uint256 balanceOther) {
        IPair mainPair = IPair(mainpair);
        (uint256 r0, uint256 r1,) = mainPair.getReserves();
        rOther = r1; rThis = r0;
        address tokenOther = usdt;
        if (tokenOther < address(this)) { rOther = r0; rThis = r1; }
        balanceOther = IERC20(tokenOther).balanceOf(mainpair);
    }
    function _isAddLiquidity(uint256 amount) internal view returns (bool) {
        (uint256 rOther, uint256 rThis, uint256 balanceOther) = _getReserves();
        uint256 amountOther;
        if (rOther > 0 && rThis > 0) amountOther = (amount * rOther) / rThis;
        return balanceOther >= rOther + amountOther;
    }
    function _isRemoveLiquidity() internal view returns (bool) {
        (uint256 rOther, , uint256 balanceOther) = _getReserves();
        return balanceOther <= rOther;
    }
    function _addLPHolder(address adr) internal {
        if (_isHolder[adr] == false && IERC20(mainpair).balanceOf(adr) > 0) {
            lpholders.push(adr);
            _isHolder[adr] = true;
        }
    }
    // reward for LP holders
    function _processReward(uint256 gas) internal {
        uint256 usdtBalance = IERC20(usdt).balanceOf(address(tokenDistributor));
        if (usdtBalance < _distributeAmount) return;

        IERC20 lpToken = IERC20(mainpair);
        uint256 lpTotal = lpToken.totalSupply();
        if (lpTotal == 0) return;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();

        address shareHolder;
        uint256 lpBalance;
        uint256 amount;

        uint256 shareholderCount = lpholders.length;

        while (gasUsed < gas && iterations < shareholderCount) {
            if (_lpidx >= shareholderCount) _lpidx = 0;
            shareHolder = lpholders[_lpidx];
            lpBalance = lpToken.balanceOf(shareHolder);

            if (lpBalance >= lpTotal.div(1000)) {
                amount = usdtBalance * lpBalance / lpTotal;
                IERC20(usdt).transferFrom(address(tokenDistributor), shareHolder, amount);
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            _lpidx++;
            iterations++;
        }
    }
}