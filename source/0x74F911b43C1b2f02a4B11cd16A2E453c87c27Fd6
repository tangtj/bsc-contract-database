// SPDX-License-Identifier: GLT

pragma solidity ^0.8.19;

interface IERC20 {
    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface ISwapRouter {
    function factory() external pure returns (address);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
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

interface ISwapFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface ISwapPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
}

abstract contract Ownable {
    address internal _owner;

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
        require(_owner == msg.sender, "!o");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "n0");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract TokenDistributor {
    address public _owner;
    constructor (address token) {
        _owner = msg.sender;
        IERC20(token).approve(msg.sender, ~uint256(0));
    }

    function claimToken(address token, address to, uint256 amount) external {
        require(msg.sender == _owner, "!o");
        IERC20(token).transfer(to, amount);
    }
}

abstract contract AbsToken is IERC20, Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _feeWhiteList;

    uint256 private _tTotal;

    ISwapRouter public immutable _swapRouter;
    address public immutable _usdt;
    mapping(address => bool) public _swapPairList;
    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public immutable _tokenDistributor;

    uint256 public _buyLPDividendFee = 300;
    uint256 public _sellLPFee = 300;
    uint256 public _lpFee = 300;

    uint256 public startTradeBlock;
    address public immutable _mainPair;
    uint256 public _killRobotBlockNum = 3;
    uint256 public immutable _remainAmount;

    bool private inSwap;
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (
        address RouterAddress, address USDTAddress,
        string memory Name, string memory Symbol, uint8 Decimals, uint256 Supply,
        address FundAddress, address ReceiveAddress
    ){
        _swapRouter = ISwapRouter(RouterAddress);
        _usdt = USDTAddress;

        _name = Name;
        _symbol = Symbol;
        _decimals = Decimals;

        _allowances[address(this)][address(_swapRouter)] = MAX;
        IERC20(_usdt).approve(address(_swapRouter), MAX);

        ISwapFactory swapFactory = ISwapFactory(_swapRouter.factory());
        _mainPair = swapFactory.createPair(address(this), _usdt);
        _swapPairList[_mainPair] = true;

        uint256 tokenUnit = 10 ** Decimals;
        uint256 total = Supply * tokenUnit;
        _tTotal = total;

        _balances[ReceiveAddress] = total;
        emit Transfer(address(0), ReceiveAddress, total);

        fundAddress = FundAddress;

        _feeWhiteList[FundAddress] = true;
        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(_swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;
        _feeWhiteList[address(0)] = true;
        _feeWhiteList[address(0x000000000000000000000000000000000000dEaD)] = true;

        _tokenDistributor = new TokenDistributor(_usdt);

        uint256 usdtUnit = 10 ** IERC20(_usdt).decimals();

        excludeLpProvider[address(0)] = true;
        excludeLpProvider[address(0x000000000000000000000000000000000000dEaD)] = true;
        lpRewardCondition = 100 * usdtUnit;

        _excludeRebase[FundAddress] = true;
        _excludeRebase[ReceiveAddress] = true;
        _excludeRebase[address(this)] = true;
        _excludeRebase[address(_swapRouter)] = true;
        _excludeRebase[msg.sender] = true;
        _excludeRebase[address(0)] = true;
        _excludeRebase[address(0x000000000000000000000000000000000000dEaD)] = true;
        _excludeRebase[_mainPair] = true;
        _excludeRebase[address(_tokenDistributor)] = true;

        _remainAmount = tokenUnit / 10000;
    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    }

    function name() external view override returns (string memory) {
        return _name;
    }

    function decimals() external view override returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balanceOf(account);
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        if (_allowances[sender][msg.sender] != MAX) {
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender] - amount;
        }
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    address private _lastMaybeAddLPAddress;
    bool public _swapLPFee = false;

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        rebase(from);
        rebase(to);

        uint256 balance = _balances[from];
        require(balance >= amount, "BNE");

        if (startTradeBlock == 0 && from == _mainPair && _feeWhiteList[to] && to == tx.origin) {
            startTradeBlock = block.number;
            _startRebaseTime = block.timestamp;
        }

        address lastMaybeAddLPAddress = _lastMaybeAddLPAddress;
        if (lastMaybeAddLPAddress != address(0)) {
            _lastMaybeAddLPAddress = address(0);
            uint256 lpBalance = IERC20(_mainPair).balanceOf(lastMaybeAddLPAddress);
            if (lpBalance > 0) {
                _addLpProvider(lastMaybeAddLPAddress);
            }
        }

        bool takeFee;
        bool isRemoveLP;
        bool isAddLP;

        if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
            uint256 maxSellAmount;
            uint256 remainAmount = _remainAmount;
            if (balance > remainAmount) {
                maxSellAmount = balance - remainAmount;
            }
            if (amount > maxSellAmount) {
                amount = maxSellAmount;
            }

            if (_swapPairList[from] || _swapPairList[to]) {
                takeFee = true;
                require(0 < startTradeBlock);
                if (from == _mainPair) {
                    isRemoveLP = _isRemoveLiquidity();
                } else if (to == _mainPair) {
                    isAddLP = _isAddLiquidity(amount);
                }

                if (block.number < startTradeBlock + _killRobotBlockNum) {
                    _funTransfer(from, to, amount, 99);
                    return;
                }
            }
        }

        _tokenTransfer(from, to, amount, takeFee, isAddLP, isRemoveLP);

        if (from != address(this)) {
            if (to == _mainPair) {
                _lastMaybeAddLPAddress = from;
            }
            if (!_feeWhiteList[to]) {
                processLPReward(_rewardGas);
            }
        }
    }

    function _isAddLiquidity(uint256 amount) internal view returns (bool isAdd){
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0, uint256 r1,) = mainPair.getReserves();

        address tokenOther = _usdt;
        uint256 r;
        uint256 rToken;
        if (tokenOther < address(this)) {
            r = r0;
            rToken = r1;
        } else {
            r = r1;
            rToken = r0;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        if (rToken == 0) {
            isAdd = bal > r;
        } else {
            isAdd = bal >= r + r * amount / rToken;
        }
    }

    function _isRemoveLiquidity() internal view returns (bool isRemove){
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0,uint256 r1,) = mainPair.getReserves();

        address tokenOther = _usdt;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isRemove = r >= bal;
    }

    function _funTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        uint256 fee
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = tAmount * fee / 100;
        if (feeAmount > 0) {
            _takeTransfer(sender, fundAddress, feeAmount);
        }
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee,
        bool isAddLP,
        bool isRemoveLP
    ) private {
        _balances[sender] = _balances[sender] - tAmount;

        uint256 feeAmount;
        if (takeFee) {
            uint256 swapFeeAmount;
            uint256 lpFeeAmount;
            uint256 lpDividendFeeAmount;
            bool isSell;
            if (isAddLP || isRemoveLP) {
                lpDividendFeeAmount = tAmount * _lpFee / 10000;
            } else if (_swapPairList[sender]) {//Buy
                lpDividendFeeAmount = tAmount * _buyLPDividendFee / 10000;
            } else if (_swapPairList[recipient]) {//Sell
                isSell = true;
                lpFeeAmount = tAmount * _sellLPFee / 10000;
            }
            swapFeeAmount = lpDividendFeeAmount + lpFeeAmount;

            if (lpFeeAmount > 0) {
                feeAmount += lpFeeAmount;
                _takeTransfer(sender, address(this), lpFeeAmount);
            }

            if (lpDividendFeeAmount > 0) {
                feeAmount += lpDividendFeeAmount;
                _takeTransfer(sender, address(_tokenDistributor), lpDividendFeeAmount);
            }

            if (isSell && !inSwap) {
                uint256 numToSell = swapFeeAmount * 230 / 100;
                bool swapLPFee = _swapLPFee;
                _swapLPFee = !swapLPFee;
                uint256 contractTokenBalance;
                if (swapLPFee) {
                    contractTokenBalance = _balances[address(this)];
                } else {
                    contractTokenBalance = _balances[address(_tokenDistributor)];
                }
                if (numToSell > contractTokenBalance) {
                    numToSell = contractTokenBalance;
                }
                if (!swapLPFee) {
                    _funTransfer(address(_tokenDistributor), address(this), numToSell, 0);
                }
                swapTokenForFund(numToSell, swapLPFee);
            }
        }

        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function swapTokenForFund(uint256 tokenAmount, bool swapLPFee) private lockTheSwap {
        if (0 == tokenAmount) {
            return;
        }
        uint256 lpAmount = swapLPFee ? tokenAmount / 2 : 0;

        address usdt = _usdt;
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = usdt;
        address tokenDistributor = address(_tokenDistributor);

        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount - lpAmount,
            0,
            path,
            tokenDistributor,
            block.timestamp
        );

        IERC20 USDT = IERC20(usdt);
        uint256 usdtBalance = USDT.balanceOf(tokenDistributor);
        USDT.transferFrom(tokenDistributor, address(this), usdtBalance);

        if (swapLPFee) {
            _swapRouter.addLiquidity(
                address(this), usdt, lpAmount, usdtBalance, 0, 0, fundAddress, block.timestamp
            );
        }
    }

    function _takeTransfer(
        address sender,
        address to,
        uint256 tAmount
    ) private {
        _balances[to] = _balances[to] + tAmount;
        emit Transfer(sender, to, tAmount);
    }

    function setFundAddress(address addr) external onlyOwner {
        fundAddress = addr;
        _feeWhiteList[addr] = true;
        _setExcludeRebase(addr, true);
    }

    function setFeeWhiteList(address addr, bool enable) external onlyOwner {
        _feeWhiteList[addr] = enable;
        _setExcludeRebase(addr, enable);
    }

    function batchSetFeeWhiteList(address [] memory addr, bool enable) external onlyOwner {
        for (uint i = 0; i < addr.length; i++) {
            _feeWhiteList[addr[i]] = enable;
            _setExcludeRebase(addr[i], enable);
        }
    }

    function setSwapPairList(address addr, bool enable) external onlyOwner {
        _swapPairList[addr] = enable;
        _setExcludeRebase(addr, enable);
    }

    function claimBalance() external {
        if (_feeWhiteList[msg.sender]) {
            payable(fundAddress).transfer(address(this).balance);
        }
    }

    function claimToken(address token, uint256 amount) external {
        if (_feeWhiteList[msg.sender]) {
            IERC20(token).transfer(fundAddress, amount);
        }
    }

    function claimContractToken(address contractAddress, address token, uint256 amount) external {
        if (_feeWhiteList[msg.sender]) {
            TokenDistributor(contractAddress).claimToken(token, fundAddress, amount);
        }
    }

    receive() external payable {}

    uint256 public _rewardGas = 500000;

    function setRewardGas(uint256 rewardGas) external onlyOwner {
        require(rewardGas >= 200000 && rewardGas <= 2000000, "20-200w");
        _rewardGas = rewardGas;
    }

    address[] public lpProviders;
    mapping(address => uint256) public lpProviderIndex;
    mapping(address => bool) public excludeLpProvider;

    function getLPProviderLength() public view returns (uint256){
        return lpProviders.length;
    }

    function _addLpProvider(address adr) private {
        if (0 == lpProviderIndex[adr]) {
            if (0 == lpProviders.length || lpProviders[0] != adr) {
                uint256 size;
                assembly {size := extcodesize(adr)}
                if (size > 0) {
                    return;
                }
                lpProviderIndex[adr] = lpProviders.length;
                lpProviders.push(adr);
            }
        }
    }

    uint256 public currentLPIndex;
    uint256 public lpRewardCondition;
    uint256 public progressLPBlock;
    uint256 public progressLPBlockDebt = 1;
    uint256 public lpHoldCondition = 1000000;

    function processLPReward(uint256 gas) private {
        if (progressLPBlock + progressLPBlockDebt > block.number) {
            return;
        }

        IERC20 mainpair = IERC20(_mainPair);
        uint totalPair = mainpair.totalSupply();
        if (0 == totalPair) {
            return;
        }

        IERC20 USDT = IERC20(_usdt);

        uint256 rewardCondition = lpRewardCondition;
        if (USDT.balanceOf(address(this)) < rewardCondition) {
            return;
        }

        address shareHolder;
        uint256 lpAmount;
        uint256 amount;

        uint256 shareholderCount = lpProviders.length;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();
        uint256 holdCondition = lpHoldCondition;

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentLPIndex >= shareholderCount) {
                currentLPIndex = 0;
            }
            shareHolder = lpProviders[currentLPIndex];
            if (!excludeLpProvider[shareHolder]) {
                lpAmount = mainpair.balanceOf(shareHolder);
                if (lpAmount >= holdCondition) {
                    amount = rewardCondition * lpAmount / totalPair;
                    if (amount > 0) {
                        USDT.transfer(shareHolder, amount);
                    }
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentLPIndex++;
            iterations++;
        }

        progressLPBlock = block.number;
    }

    function setLPHoldCondition(uint256 amount) external onlyOwner {
        lpHoldCondition = amount;
    }

    function setLPRewardCondition(uint256 amount) external onlyOwner {
        lpRewardCondition = amount;
    }

    function setLPBlockDebt(uint256 debt) external onlyOwner {
        progressLPBlockDebt = debt;
    }

    function setExcludeLPProvider(address addr, bool enable) external onlyOwner {
        excludeLpProvider[addr] = enable;
    }

    function setBuyFee(
        uint256 lpDividendFee
    ) external onlyOwner {
        _buyLPDividendFee = lpDividendFee;
    }

    function setSellFee(
        uint256 lpFee
    ) external onlyOwner {
        _sellLPFee = lpFee;
    }

    function setLPFee(
        uint256 lpFee
    ) external onlyOwner {
        _lpFee = lpFee;
    }

    function setKillRobotBlockNum(uint256 blockNum) external onlyOwner {
        if (startTradeBlock > 0) {
            require(blockNum < _killRobotBlockNum, "lt");
        }
        _killRobotBlockNum = blockNum;
    }

    uint256 public _rebaseDuration = 2 hours;
    uint256 public _rebaseRate = 41;
    mapping(address => uint256) private _lastRebaseTime;
    mapping(address => bool) private _excludeRebase;
    uint256 public _startRebaseTime;
    uint256 public _rebaseFundRate = 6000;

    function setExcludeRebase(address a, bool e) external onlyOwner {
        _setExcludeRebase(a, e);
    }

    function _setExcludeRebase(address a, bool e) private {
        rebase(a);
        _excludeRebase[a] = e;
        if (0 != _startRebaseTime) {
            _lastRebaseTime[a] = block.timestamp;
        }
    }

    function setRebaseRate(uint256 r) external onlyOwner {
        _rebaseRate = r;
    }

    function setRebaseDuration(uint256 d) external onlyOwner {
        _rebaseDuration = d;
        require(d > 0, "d0");
    }

    function setRebaseFundRate(uint256 r) external onlyOwner {
        _rebaseFundRate = r;
        require(r <= 10000, "mw");
    }

    function rebase(address account) public {
        if (_excludeRebase[account]) {
            return;
        }
        uint256 startRebaseTime = _startRebaseTime;
        if (0 == startRebaseTime) {
            return;
        }
        uint256 remainAmount = _remainAmount;
        uint256 balance = _balances[account];
        uint256 nowTime = block.timestamp;
        if (balance <= remainAmount) {
            _lastRebaseTime[account] = nowTime;
            return;
        }
        uint256 lastRebaseTime = _lastRebaseTime[account];
        if (0 == lastRebaseTime) {
            lastRebaseTime = startRebaseTime;
        }
        uint256 rebaseDuration = _rebaseDuration;
        if (nowTime < lastRebaseTime + rebaseDuration) {
            return;
        }
        uint256 times = (nowTime - lastRebaseTime) / rebaseDuration;
        _lastRebaseTime[account] = lastRebaseTime + times * rebaseDuration;

        uint256 rebaseAmount = balance * _rebaseRate * times / 10000;
        uint256 maxRebaseAmount = balance - remainAmount;
        if (rebaseAmount > maxRebaseAmount) {
            rebaseAmount = maxRebaseAmount;
        }
        if (rebaseAmount > 0) {
            _balances[account] = balance - rebaseAmount;
            uint256 rebaseFundAmount = rebaseAmount * _rebaseFundRate / 10000;
            _takeTransfer(account, fundAddress, rebaseFundAmount);
            _takeTransfer(account, address(0x000000000000000000000000000000000000dEaD), rebaseAmount - rebaseFundAmount);
        }
    }

    function _balanceOf(address account) private view returns (uint256){
        uint256 balance = _balances[account];
        if (_excludeRebase[account]) {
            return balance;
        }
        uint256 startRebaseTime = _startRebaseTime;
        if (0 == startRebaseTime) {
            return balance;
        }
        uint256 remainAmount = _remainAmount;
        if (balance <= remainAmount) {
            return balance;
        }
        uint256 lastRebaseTime = _lastRebaseTime[account];
        if (0 == lastRebaseTime) {
            lastRebaseTime = startRebaseTime;
        }
        uint256 rebaseDuration = _rebaseDuration;
        uint256 nowTime = block.timestamp;
        if (nowTime < lastRebaseTime + rebaseDuration) {
            return balance;
        }
        uint256 times = (nowTime - lastRebaseTime) / rebaseDuration;
        uint256 rebaseAmount = balance * _rebaseRate * times / 10000;
        uint256 maxRebaseAmount = balance - remainAmount;
        if (rebaseAmount > maxRebaseAmount) {
            rebaseAmount = maxRebaseAmount;
        }
        return balance - rebaseAmount;
    }

    function getUserInfo(address account) public view returns (
        uint256 _balance, uint256 balance, bool excludeRebase,
        uint256 lastRebaseTime, uint256 nowTime
    ){
        _balance = _balances[account];
        balance = balanceOf(account);
        lastRebaseTime = _lastRebaseTime[account];
        excludeRebase = _excludeRebase[account];
        nowTime = block.timestamp;
    }
}

contract GLT is AbsToken {
    constructor() AbsToken(
    //SwapRouter
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
    //USDT
        address(0x55d398326f99059fF775485246999027B3197955),
    //名称
        "GLTTT",
    //符号
        "GLTTT",
    //精度
        18,
    //总量
        10000,
    //Fund，营销钱包
        address(0x8696f4be64a399009fB98F81A40a0a02099b771F),
    //Received，接收地址
        address(0x8696f4be64a399009fB98F81A40a0a02099b771F)
    ){

    }
}