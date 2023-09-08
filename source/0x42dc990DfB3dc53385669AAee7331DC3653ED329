// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

interface IUniswapV2Router01 {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    )
        external
        returns (
            uint256 amountA,
            uint256 amountB,
            uint256 liquidity
        );

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );

    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETH(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountToken, uint256 amountETH);

    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETHWithPermit(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountToken, uint256 amountETH);

    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapTokensForExactTokens(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactETHForTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function swapTokensForExactETH(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactTokensForETH(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapETHForExactTokens(
        uint256 amountOut,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function quote(
        uint256 amountA,
        uint256 reserveA,
        uint256 reserveB
    ) external pure returns (uint256 amountB);

    function getAmountOut(
        uint256 amountIn,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountOut);

    function getAmountIn(
        uint256 amountOut,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountIn);

    function getAmountsOut(uint256 amountIn, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);

    function getAmountsIn(uint256 amountOut, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountETH);

    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

interface IERC20Extended {
    function totalSupply() external view returns (uint256);

    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address _owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

interface IUniswapV2Pair is IERC20Extended {
    function MINIMUM_LIQUIDITY() external pure returns (uint);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: not owner");
        _;
    }

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract ERC20 is IERC20Extended, Context {
    
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;
    string private _name;
    string private _symbol;
    uint8 private _decimals = 18;

    constructor (string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    function name() public view override returns (string memory) {
        return _name;
    }

    function symbol() public view override returns (string memory) {
        return _symbol;
    }

    function decimals() public view override returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
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

    function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);
        address spender = _msgSender();
        if (_allowances[sender][spender] < _totalSupply) {
            _approve(sender, spender, _allowances[sender][spender] - amount);
        }
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        _beforeTokenTransfer(sender, recipient, amount);

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[sender] = senderBalance - amount;
        }
        _balances[recipient] = _balances[recipient] + amount;

        emit Transfer(sender, recipient, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");
        _beforeTokenTransfer(address(0), account, amount);
        _totalSupply = _totalSupply + amount;
        _balances[account] = _balances[account] + amount;
        emit Transfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");
        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
        }
        _totalSupply = _totalSupply - amount;
        emit Transfer(account, address(0), amount);
    }

    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual {}
}

contract RewardDistributor {
    constructor (address rewardToken) {
        IERC20Extended(rewardToken).approve(msg.sender, type(uint).max);
    }
}

contract LwToken is ERC20, Ownable {
    address private constant DEAD = address(0xdead);
    address private constant ZERO = address(0);
    uint256 private constant FEE_DENOMINATOR = 10000;

    IUniswapV2Router02 public router;
    address public rewardToken;
    address public pair;
    address public marketingFeeReceiver;

    uint256 public liquidityFee = 5700;     // 57%
    uint256 public buyFee = 350;
    uint256 public sellFee = 350;

    RewardDistributor public distributor;
    uint256 public distributorGas;

    bool public swapEnabled;
    uint256 public swapThreshold;
    uint256 public swapStartTime;
    uint256 public freeTime = 300;

    mapping(address => bool) public isFeeExempt;
    mapping(address => bool) public isDividendExempt;

    struct Share {
        uint256 amount;
        uint256 totalExcluded;
        uint256 totalRealised;
        uint256 lastClaimAt;
        bool isActive;
    }

    address[] public shareholders;
    mapping(address => uint256) public shareholderIndexes;
    mapping(address => Share) public shares;
    uint256 public totalDividends;
    uint256 public totalDistributed;
    uint256 public dividendsPerShare;
    uint256 public dividendsPerShareAccuracyFactor = 10**36;
    uint256 public minPeriod;
    uint256 public minDistribution;
    uint256 currentIndex;

    event DistributorCreated(address distributorAddr);
    event AddShareholder(address shareholder);
    event RemoveShareholder(address shareholder);
    event DistributeDividend(address shareholder, uint256 amount);

    bool private inSwapping;
    modifier lockTheSwap {
        inSwapping = true;
        _;
        inSwapping = false;
    }

    constructor() ERC20("LongWang", "LW") {
        _mint(_msgSender(), 10_000 * 10 ** 18);

        address routerV2 = address(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        rewardToken = address(0x55d398326f99059fF775485246999027B3197955);
        router = IUniswapV2Router02(routerV2);
        pair = IUniswapV2Factory(router.factory()).createPair(
            address(this),
            rewardToken
        );

        distributor = new RewardDistributor(rewardToken);
        minPeriod = 1 hours;
        minDistribution = 3 * (10**(IERC20Extended(rewardToken).decimals() - 1));
        distributorGas = 500000;
        swapThreshold = totalSupply() / 100000;      // 0.001%
        marketingFeeReceiver = address(0x68e749ac18b76421764ecf12a7004D3597bF6912);

        isFeeExempt[_msgSender()] = true;
        isFeeExempt[address(this)] = true;
        isFeeExempt[marketingFeeReceiver] = true;
        isFeeExempt[address(0x077a42Cce922707db10B23A9259E3fE2fc7B39F7)] = true;
        isFeeExempt[address(0x861b2993c46851129D33338D53312f19b1dd6fDd)] = true;
        isFeeExempt[address(0x93785C67340185545575d28F7E35398c7306cBeD)] = true;
        isFeeExempt[address(0x0eB8bF66BBCcADe4fdE7580aCfDab3E3256FeF96)] = true;
        isFeeExempt[address(0xCc9F4078BEA7494d1204EcAd01b6Ed72b6AC2cB6)] = true;
        isFeeExempt[address(0xDAF2db2a9E6297dBA12253005bc5b5986168f8F4)] = true;

        isDividendExempt[address(this)] = true;
        isDividendExempt[routerV2] = true;
        isDividendExempt[pair] = true;
        isDividendExempt[ZERO] = true;
        isDividendExempt[DEAD] = true;

        _approve(address(this), routerV2, type(uint).max);
        IERC20Extended(rewardToken).approve(routerV2, type(uint).max);

        emit DistributorCreated(address(distributor));
    }

    receive() external payable {}

    function getCurrentFees() public view returns (
        uint256 _liquidityFee,
        uint256 _buyFee,
        uint256 _sellFee
    ) {
        if (swapEnabled && swapStartTime > 0) {
            if (block.timestamp > swapStartTime + freeTime) {
                _liquidityFee = liquidityFee;
                _buyFee = buyFee;
                _sellFee = sellFee;
            } else {
                _liquidityFee = 1300;
                _buyFee = 1300;
                _sellFee = 1300;
            }
        }
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal override {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        if (!isFeeExempt[sender] && !isFeeExempt[recipient]) {
            require(swapEnabled, "Trading not open");
        }

        if (amount == 0) {
            super._transfer(sender, recipient, 0);
            return;
        }

        uint256 contractTokenBalance = balanceOf(address(this));
        bool needSwap = contractTokenBalance >= swapThreshold;
        if (block.timestamp < swapStartTime + freeTime) {
            needSwap = contractTokenBalance > 0;
        }
        if (
            needSwap &&
            !inSwapping &&
            recipient == pair &&
            !isFeeExempt[sender] &&
            !isFeeExempt[recipient]
        ) {
            _swapAndDividend(contractTokenBalance);
        }

        bool takeFee = true;
        if (isFeeExempt[sender] || isFeeExempt[recipient]) {
            takeFee = false;
        }
        if (takeFee) {
            uint256 feeAmt;
            (, uint256 _buyFee, uint256 _sellFee) = getCurrentFees();
            if (recipient == pair) {
                feeAmt = amount * _sellFee / FEE_DENOMINATOR;
            } else if (sender == pair) {
                feeAmt = amount * _buyFee / FEE_DENOMINATOR;
            }

            amount = amount - feeAmt;
            super._transfer(sender, address(this), feeAmt);
        }
        super._transfer(sender, recipient, amount);

        if (!isDividendExempt[sender]) {
            refreshShare(sender, false);
        }
        if (!isDividendExempt[recipient]) {
            refreshShare(recipient, false);
        }

        if (sender != address(this)) {
            processReward();
        }
    }

    function _swapAndDividend(uint256 tokens) private lockTheSwap {
        _swapTokensForRewardToken(tokens);
        uint256 currentBalance = IERC20Extended(rewardToken).balanceOf(address(distributor));

        (uint256 _liquidityFee,,) = getCurrentFees();
        uint256 forLiquidity = currentBalance * _liquidityFee / FEE_DENOMINATOR;
        uint256 forMarket = currentBalance - forLiquidity;
        IERC20Extended(rewardToken).transferFrom(address(distributor), marketingFeeReceiver, forMarket);
        IERC20Extended(rewardToken).transferFrom(address(distributor), address(this), forLiquidity);

        totalDividends += forLiquidity;
        IUniswapV2Pair pairToken = IUniswapV2Pair(pair);
        uint256 totalShares = pairToken.totalSupply();
        if (totalShares > pairToken.MINIMUM_LIQUIDITY()) {
            dividendsPerShare = dividendsPerShare + (
                dividendsPerShareAccuracyFactor * forLiquidity / (totalShares - pairToken.MINIMUM_LIQUIDITY())
            );
        } else {
            dividendsPerShare = 0;
        }
    }

    function _swapTokensForRewardToken(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = rewardToken;

        // make the swap
        router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount
            path,
            address(distributor),
            block.timestamp
        );
    }

    function refreshShare(address shareholder, bool isClear) internal {
        if (isClear) {
            removeShareholder(shareholder);
        } else {
            uint256 newShare = IUniswapV2Pair(pair).balanceOf(shareholder);
            if (newShare > 0) {
                addShareholder(shareholder);
            } else {
                removeShareholder(shareholder);
            }
            
            if (shares[shareholder].isActive && shares[shareholder].amount == 0) {
                shares[shareholder].amount = newShare;
                shares[shareholder].totalExcluded = getCumulativeDividends(newShare);
            }
        }
    }

    function processReward() internal {
        uint256 shareholderCount = shareholders.length;

        if (shareholderCount == 0) {
            return;
        }

        uint256 gasUsed = 0;
        uint256 gasLeft = gasleft();
        uint256 iterations = 0;

        while (gasUsed < distributorGas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }

            if (shouldDistribute(shareholders[currentIndex])) {
                distributeDividend(shareholders[currentIndex]);
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }
    }

    function shouldDistribute(address shareholder)
        internal
        view
        returns (bool)
    {
        return
            shares[shareholder].lastClaimAt + minPeriod < block.timestamp &&
            getUnpaidEarnings(shareholder) > minDistribution;
    }

    function distributeDividend(address shareholder) internal {
        uint256 amount = getUnpaidEarnings(shareholder);
        if (amount > 0) {
            totalDistributed += amount;
            if (IERC20Extended(rewardToken).balanceOf(address(this)) >= amount) {
                IERC20Extended(rewardToken).transfer(shareholder, amount);
                shares[shareholder].lastClaimAt = block.timestamp;
                shares[shareholder].totalRealised += amount;

                emit DistributeDividend(shareholder, amount);

                uint256 newShare = IUniswapV2Pair(pair).balanceOf(shareholder);
                shares[shareholder].amount = newShare;
                shares[shareholder].totalExcluded = getCumulativeDividends(newShare);
            }
        }
    }

    function claimDividend() external {
        distributeDividend(msg.sender);
    }

    function getUnpaidEarnings(address shareholder)
        public
        view
        returns (uint256)
    {
        if (!shares[shareholder].isActive || shares[shareholder].amount == 0) {
            return 0;
        }

        uint256 shareholderTotalDividends = getCumulativeDividends(shares[shareholder].amount);
        uint256 shareholderTotalExcluded = shares[shareholder].totalExcluded;

        if (shareholderTotalDividends <= shareholderTotalExcluded) {
            return 0;
        }

        return shareholderTotalDividends - shareholderTotalExcluded;
    }

    function getCumulativeDividends(uint256 share)
        internal
        view
        returns (uint256)
    {
        return share * dividendsPerShare / dividendsPerShareAccuracyFactor;
    }

    function addShareholder(address shareholder) internal {
        if (!shares[shareholder].isActive) {
            shareholderIndexes[shareholder] = shareholders.length;
            shareholders.push(shareholder);
            shares[shareholder].isActive = true;

            emit AddShareholder(shareholder);
        }
    }

    function removeShareholder(address shareholder) internal {
        if (shares[shareholder].isActive) {
            shareholders[shareholderIndexes[shareholder]] = shareholders[
                shareholders.length - 1
            ];
            shareholderIndexes[
                shareholders[shareholders.length - 1]
            ] = shareholderIndexes[shareholder];
            shareholders.pop();
            shares[shareholder].isActive = false;

            emit RemoveShareholder(shareholder);
        }
    }

    function setIsDividendExempt(address holder, bool exempt)
        external
        onlyOwner
    {
        isDividendExempt[holder] = exempt;
        refreshShare(holder, exempt);
    }

    function setIsFeeExempt(address holder, bool exempt) external onlyOwner {
        isFeeExempt[holder] = exempt;
    }

    function setFees(
        uint256 _liquidityFee,
        uint256 _buyFee,
        uint256 _sellFee
    ) external onlyOwner {
        require(_liquidityFee < 10000, "Liquidity fee too high");
        require(_buyFee < 2000, "Buy fee too high");
        require(_sellFee < 2000, "Sell fee too high");
        liquidityFee = _liquidityFee;
        buyFee = _buyFee;
        sellFee = _sellFee;
    }

    function setFeeReceiver(
        address _feeReceiver
    ) external onlyOwner {
        marketingFeeReceiver = _feeReceiver;
    }

    function setSwapSettings(
        bool _enabled,
        uint256 _amount
    ) external onlyOwner {
        if (swapEnabled == false && _enabled == true) {
            swapEnabled = true;
            swapStartTime = block.timestamp;
        }
        if (_amount > 0) {
            swapThreshold = _amount;
        }
    }

    function setDistributorSettings(
        uint256 _minPeriod,
        uint256 _minDistribution,
        uint256 gas
    ) external onlyOwner {
        minPeriod = _minPeriod;
        minDistribution = _minDistribution;
        require(gas <= 5000000, "Gas must be lower than 5000000");
        distributorGas = gas;
    }
}