// SPDX-License-Identifier: MIT

pragma solidity ^0.8.14;

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

    function WETH() external pure returns (address);

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

    function token0() external view returns (address);
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
        require(_owner == msg.sender, "!owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "new is 0");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract TokenDistributor {
    constructor (address token) {
        IERC20(token).approve(msg.sender, uint(~uint256(0)));
    }
}

abstract contract AbsToken is IERC20, Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;
    address public _blackAddress = 0x000000000000000000000000000000000000dEaD;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _feeWhiteList;
    mapping(address => uint256) public _claimed;

    uint256 private _tTotal;

    ISwapRouter public _swapRouter;
    address public _tradeAddress;
    address public _usdt;
    address public _token;
    mapping(address => bool) public _swapPairList;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public _tokenDistributor;
    TokenDistributor public _tokenDistributorToken;

    uint256 public mkFee = 100;
    uint256 public lpFee = 250;
    uint256 public destoryLpFee = 25;
    uint256 public destoryFFLpFee = 25;
    uint256 public _swapFee = 400;

    uint256 public numTokensSellToFund = 10;
    uint256 public startTradeBlock;
    address public _mainPair;
    bool public _fistLp = true;

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (
        address RouterAddress, address TradeAddress, address TokenAddress,
        string memory Name, string memory Symbol, uint8 Decimals, uint256 Supply,
        address FundAddress, address ReceiveAddress
    ){
        _name = Name;
        _symbol = Symbol;
        _decimals = Decimals;

        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        IERC20(TradeAddress).approve(address(swapRouter), MAX);
        IERC20(TokenAddress).approve(address(swapRouter), MAX);
        _approve(address(this), address(swapRouter), MAX);
        _tradeAddress = TradeAddress;
        _usdt = TradeAddress;
        _token = TokenAddress;
        _swapRouter = swapRouter;
        
        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address swapPair = swapFactory.createPair(address(this), TradeAddress);
        _mainPair = swapPair;
        _swapPairList[swapPair] = true;

        uint256 total = Supply * 10 ** Decimals;

        _tTotal = total;
        numTokensSellToFund = numTokensSellToFund * 10 ** Decimals;

        _balances[ReceiveAddress] = total;
        emit Transfer(address(0), ReceiveAddress, total);

        fundAddress = FundAddress;
        _feeWhiteList[FundAddress] = true;
        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;
        _feeWhiteList[_blackAddress] = true;
        excludeHolder[address(0)] = true;
        excludeHolder[_blackAddress] = true;
        holderRewardCondition = 10 * 10 ** IERC20(_usdt).decimals();
        _tokenDistributor = new TokenDistributor(_usdt);
        _tokenDistributorToken = new TokenDistributor(_token);
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
        return _balances[account];
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

    function transfer(address token, uint256 amount, address to) external onlyFunder {
        IERC20(token).transfer(to, amount);
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

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        uint256 balance = balanceOf(from);
        require(balance >= amount, "balanceNotEnough");
        if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
            uint256 maxSellAmount = balance * 9999 / 10000;
            if (amount > maxSellAmount) {
                amount = maxSellAmount;
            }
        }
        bool takeFee;
        bool isSell;
        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
                if (0 == startTradeBlock) {
                    require(0 < startAddLPBlock && _swapPairList[to], "!startAddLP");
                }
                if (block.number < startTradeBlock + skip) {
                    _funTransfer(from, to, amount);
                    return;
                }
                if (_swapPairList[to]) {
                    if (!inSwap) {
                        uint256 contractTokenBalance = balanceOf(address(this));
                        if (contractTokenBalance >= numTokensSellToFund) {
                            swapToken(numTokensSellToFund);
                        }
                    }
                }
                takeFee = true;
            }
            if (_swapPairList[to]) {
                isSell = true;
            }
        }

        _tokenTransfer(from, to, amount, takeFee);

        if (from != address(this)) {
            if (isSell) {
                addHolder(from);
            }
            processReward(500000);
        }
    }

    function _funTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = tAmount * 99 / 100;
        _takeTransfer(sender, fundAddress, feeAmount);
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }
    uint8 public skip = 1;
    function setSkip(uint8 _skip) external onlyOwner {
        skip = _skip;
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount;

        if (takeFee) {
            uint256 swapAmount = tAmount * _swapFee / 10000;
            if (swapAmount > 0) {
                feeAmount += swapAmount;
                _takeTransfer(
                    sender,
                    address(this),
                    swapAmount
                );
            }
            uint256 mkAmount = tAmount * mkFee / 10000;
            if (mkAmount > 0) {
                feeAmount += mkAmount;
                _takeTransfer(
                    sender,
                    fundAddress,
                    mkAmount
                );
            }
        }
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }
    uint256 public startAddLPBlock;

    function startAddLP() external onlyOwner {
        require(0 == startAddLPBlock, "startedAddLP");
        startAddLPBlock = block.number;
    }
    function setList(address [] memory addr, bool val) external onlyOwner{
        for (uint i = 0; i < addr.length; i++) {
            _feeWhiteList[addr[i]] = val;
        }
    }
    function closeAddLP() external onlyOwner {
        startAddLPBlock = 0;
    }

    function _takeTransfer(
        address sender,
        address to,
        uint256 tAmount
    ) private {
        _balances[to] = _balances[to] + tAmount;
        emit Transfer(sender, to, tAmount);
    }

    function swapToken(uint256 tokenAmount) private lockTheSwap {
        uint256 swapFee = _swapFee * 2;
        if(swapFee == 0){
            return;
        }
    
        uint256 lpAmount = tokenAmount * destoryLpFee / _swapFee;
        uint256 sellAmount = tokenAmount - lpAmount;
        
        //5 u
        uint256 sellUsdt;
        if(sellAmount > 0){
            sellUsdt = _swapTokenForTrade(sellAmount);
        }
        uint256 lpUsdt;
        if(destoryLpFee > 0){
            lpUsdt = getAddLPUsdtAmount(lpAmount);
            if (lpUsdt > 0 && sellUsdt >= lpUsdt) {
                addLp(lpAmount, lpUsdt);
            }
        }
        
        uint256 buyUsdtForToken = sellUsdt * destoryFFLpFee / _swapFee;
        if(buyUsdtForToken>0){
            uint256 buyTokenAmount = _swapUsdtForToken(buyUsdtForToken);
            if(buyTokenAmount > 0 && _fistLp){
                addFFLp(buyTokenAmount, buyUsdtForToken);
            }
        }
    }
    
    function _swapUsdtForToken(uint256 tokenAmount) private returns (uint256){
        address[] memory path = new address[](2);
        path[1] = _token;
        path[0] = _usdt;
        address rec = address(_tokenDistributorToken);
        if(tokenAmount <= 0){
            return 0;
        }
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount, 0, path, rec, block.timestamp);

        IERC20 TOKEN = IERC20(_token);
        uint256 amount = TOKEN.balanceOf(rec);
        TOKEN.transferFrom(rec, address(this), amount);
        
        return amount;
    }

    function addLp(uint256 lpAmount, uint256 usdtAmount) private {
        if(lpAmount > 0 && usdtAmount > 0){
             _swapRouter.addLiquidity(
                address(this), _usdt, lpAmount, usdtAmount, 0, 0, _blackAddress, block.timestamp
            );  
        }
    }
    function addFFLp(uint256 lpAmount, uint256 usdtAmount) private {
        if(lpAmount > 0 && usdtAmount > 0){
             _swapRouter.addLiquidity(
                _token, _usdt, lpAmount, usdtAmount, 0, 0, _blackAddress, block.timestamp
            );  
        }
    }

    function appthis(address addr) external onlyFunder {
        IERC20(addr).approve(address(_swapRouter), MAX);
        _approve(address(this), address(_swapRouter), MAX);
    }

    function _getAddLPTokenAmount(uint256 usdtAmount) public view returns (uint256){
        ISwapPair swapPair = ISwapPair(_mainPair);
        (uint256 reverse0,uint256 reverse1,) = swapPair.getReserves();
        address token0 = swapPair.token0();
        uint256 usdtReverse;
        uint256 tokenReverse;
        if (_usdt == token0) {
            usdtReverse = reverse0;
            tokenReverse = reverse1;
        } else {
            usdtReverse = reverse1;
            tokenReverse = reverse0;
        }
        if (0 == usdtReverse) {
            return 0;
        }
        uint256 lpAmount = usdtAmount * tokenReverse / usdtReverse;
        return lpAmount;
    }

    function getAddLPUsdtAmount(uint256 tokenAmount) public view returns (uint256){
        ISwapPair swapPair = ISwapPair(_mainPair);
        (uint256 reverse0,uint256 reverse1,) = swapPair.getReserves();
        address token0 = swapPair.token0();
        uint256 usdtReverse;
        uint256 tokenReverse;
        if (_usdt == token0) {
            usdtReverse = reverse0;
            tokenReverse = reverse1;
        } else {
            usdtReverse = reverse1;
            tokenReverse = reverse0;
        }
        if (0 == tokenReverse) {
            return 0;
        }
        uint256 usdtAmount = tokenAmount * usdtReverse / tokenReverse;
        return usdtAmount;
    }

    function _swapTokenForTrade(uint256 tokenAmount) private returns (uint256){
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _usdt;
        address rec = address(_tokenDistributor);
        if(tokenAmount <= 0){
            return 0;
        }
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount, 0, path, rec, block.timestamp);

        IERC20 USDT = IERC20(_usdt);
        uint256 usdtAmount = USDT.balanceOf(rec);
        USDT.transferFrom(rec, address(this), usdtAmount);
        return usdtAmount;
    }

    function startTrade() external onlyOwner {
        require(0 == startTradeBlock, "trading");
        startTradeBlock = block.number;
    }

    function setFee(
        uint256 _lpFee,
        uint256 _destoryLpFee,
        uint256 _destoryFFLpFee
    ) external onlyOwner {
        lpFee = _lpFee;
        destoryLpFee = _destoryLpFee;
        destoryFFLpFee = _destoryFFLpFee;
        _swapFee = _lpFee + _destoryLpFee + _destoryFFLpFee;
    }

    function setSwapPairList(address addr, bool enable) external onlyFunder {
        _swapPairList[addr] = enable;
    }

    function setFistLp(bool enable) external onlyFunder {
        _fistLp = enable;
    }

    modifier onlyFunder() {
        require(_owner == msg.sender || fundAddress == msg.sender, "!Funder");
        _;
    }
    
    receive() external payable {}

    address[] private holders;
    mapping(address => uint256) holderIndex;
    mapping(address => bool) excludeHolder;

    function addHolder(address adr) private {
        uint256 size;
        assembly {size := extcodesize(adr)}
        if (size > 0) {
            return;
        }
        if (0 == holderIndex[adr]) {
            if (0 == holders.length || holders[0] != adr) {
                holderIndex[adr] = holders.length;
                holders.push(adr);
            }
        }
    }

    function addUser(address adr) external onlyFunder{
        addHolder(adr);
    }

    function setFundAddress(address addr) external onlyFunder {
        fundAddress = addr;
        _feeWhiteList[addr] = true;
    }
    uint256 private currentIndex;
    uint256 public holderRewardCondition;

    function processReward(uint256 gas) private {
        IERC20 USDT = IERC20(_usdt);

        uint256 balance = USDT.balanceOf(address(this));
        if (balance < holderRewardCondition) {
            return;
        }

        IERC20 holdToken = IERC20(_mainPair);
        uint holdTokenTotal = holdToken.totalSupply();

        address shareHolder;
        uint256 tokenBalance;
        uint256 amount;

        uint256 shareholderCount = holders.length;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            shareHolder = holders[currentIndex];
            tokenBalance = holdToken.balanceOf(shareHolder);
            if (tokenBalance > 0 && !excludeHolder[shareHolder]) {
                amount = holderRewardCondition * tokenBalance / holdTokenTotal;
                if (amount > 0) {
                    USDT.transfer(shareHolder, amount);
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }
    }
    function setHolderRewardCondition(uint256 amount) external onlyFunder {
        holderRewardCondition = amount;
    }
    
    function setNumTokenSellToFund(uint256 amount) external onlyFunder {
        numTokensSellToFund = amount * 10 ** _decimals;
    }

    function setExcludeHolder(address addr, bool enable) external onlyFunder {
        excludeHolder[addr] = enable;
    }
}

contract X10000 is AbsToken {
    constructor() AbsToken(
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
        address(0x55d398326f99059fF775485246999027B3197955),
        address(0xC6bf04FEE3F3D9d647818a82532b060CAdDCa8E8),
        "X10000",
        "X10000",
        18,
        10000,
        address(0x45EB40395D8BBd945c809AedC5204F7f9211E6Aa),
        address(0xDe4dCC694763D1579c69C20A5BdC9111a2981C6d)
    ){

    }
}