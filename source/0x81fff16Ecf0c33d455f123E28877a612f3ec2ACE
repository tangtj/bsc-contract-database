/**
 *Submitted for verification at BscScan.com on 2023-03-20
 */

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.18;

interface IERC20 {
    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

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
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);

    function feeTo() external view returns (address);
}

interface ISwapPair {
    function getReserves()
        external
        view
        returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function totalSupply() external view returns (uint);

    function kLast() external view returns (uint);
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
        IERC20(token).approve(msg.sender, uint(~uint256(0)));
    }
}

library Math {
    function min(uint x, uint y) internal pure returns (uint z) {
        z = x < y ? x : y;
    }

    function sqrt(uint y) internal pure returns (uint z) {
        if (y > 3) {
            z = y;
            uint x = y / 2 + 1;
            while (x < z) {
                z = x;
                x = (y / x + x) / 2;
            }
        } else if (y != 0) {
            z = 1;
        }
    }
}

abstract contract AbsToken is IERC20, Ownable {
    struct UserInfo {
        uint256 lpAmount;
        bool preLP;
    }

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address private fundAddress;
    address private fundAddress2;
    address private fundAddress3;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) private _feeWhiteList;
    mapping(address => bool) public _blackList;
    mapping(address => bool) public _bWList;

    uint256 private _tTotal;

    ISwapRouter private immutable _swapRouter;
    address private immutable _usdt;
    TokenDistributor private immutable _tokenDistributor;
    address public immutable _mainPair;

    mapping(address => bool) private _swapPairList;
    mapping(address => address) public upper;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);

    uint256 public _buyDestroyFee = 200; //销毁
    uint256 public _buyFundFee = 150; //BOT
    uint256 public _buyFundFee2 = 100; //护盘
    uint256 public _buyFundFee3 = 0;
    uint256 public _buyLPDividendFee = 150;
    uint256 public _buyLPFee = 0;
    uint256 public _buyBuybackFee = 0;
    uint256 public _buyTeamFee = 400;

    uint256 public _sellDestroyFee = 200; //销毁
    uint256 public _sellFundFee = 150; //BOT
    uint256 public _sellFundFee2 = 100; //护盘
    uint256 public _sellFundFee3 = 0;
    uint256 public _sellLPDividendFee = 150;
    uint256 public _sellLPFee = 0;
    uint256 public _sellBuybackFee = 0;
    uint256 public _sellTeamFee = 400;

    uint256 public _transferFee = 0;
    uint256[] public teamRate = [500, 125, 125, 125, 125];
    uint256 public bindAmount = 1 * 1e18;

    uint256 public startTradeBlock;
    uint256 public startTradeTime;
    uint256 public startAddLPBlock;
    uint256 public startBWBlock;

    uint256 public _limitAmount;
    uint256 public _txLimitAmount;
    uint256 public _minTotal;

    address public _receiveAddress;

    uint256 public _airdropLen = 0;
    uint256 public _airdropAmount = 0;

    uint256 public _removeLPFee = 0;
    uint256 public _addLPFee = 0;
    address public _lpFeeReceiver;

    uint256 private constant _killBlock = 3;

    address public _buybackToken;
    mapping(address => UserInfo) private _userInfo;

    uint256 public _lpFeePerTime = 500;
    uint256 public _lpFeeTimeDuration = 24 hours;

    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor(
        address RouterAddress,
        address USDTAddress,
        address BuybackToken,
        string memory Name,
        string memory Symbol,
        uint8 Decimals,
        uint256 Supply,
        address FundAddress,
        address FundAddress2,
        address FundAddress3,
        address ReceiveAddress,
        uint256 LimitAmount,
        uint256 MinTotal,
        uint256 TxLimitAmount
    ) {
        _name = Name;
        _symbol = Symbol;
        _decimals = Decimals;

        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        address usdt = USDTAddress;
        IERC20(usdt).approve(address(swapRouter), MAX);

        _usdt = usdt;
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address usdtPair = swapFactory.createPair(address(this), usdt);
        _swapPairList[usdtPair] = true;
        _mainPair = usdtPair;
        _buybackToken = BuybackToken;

        uint256 total = Supply * 10 ** Decimals;
        _tTotal = total;

        _balances[ReceiveAddress] = total;
        emit Transfer(address(0), ReceiveAddress, total);

        _receiveAddress = ReceiveAddress;
        _lpFeeReceiver = ReceiveAddress;
        fundAddress = FundAddress;
        fundAddress2 = FundAddress2;
        fundAddress3 = FundAddress3;

        _feeWhiteList[FundAddress] = true;
        _feeWhiteList[FundAddress2] = true;
        _feeWhiteList[FundAddress3] = true;
        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;
        _feeWhiteList[address(0)] = true;
        _feeWhiteList[
            address(0x000000000000000000000000000000000000dEaD)
        ] = true;

        _limitAmount = LimitAmount * 10 ** Decimals;
        _txLimitAmount = TxLimitAmount * 10 ** Decimals;

        _tokenDistributor = new TokenDistributor(usdt);

        _minTotal = MinTotal * 10 ** Decimals;

        excludeHolder[address(0)] = true;
        excludeHolder[
            address(0x000000000000000000000000000000000000dEaD)
        ] = true;
        uint256 usdtUnit = 10 ** IERC20(usdt).decimals();
        holderRewardCondition = 10 * usdtUnit;

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

    function transfer(
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(
        address owner,
        address spender
    ) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(
        address spender,
        uint256 amount
    ) public override returns (bool) {
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

    function _approve(address owner, address spender, uint256 amount) private {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(address from, address to, uint256 amount) private {
        if (upper[to] == address(0) && amount >= bindAmount) {
            if (
                _swapPairList[to] == false &&
                _swapPairList[from] == false &&
                to != address(0)
            ) {
                upper[to] = from;
            }
        }
        require(!_blackList[from] || _feeWhiteList[from], "blackList");

        uint256 balance = balanceOf(from);
        require(balance >= amount, "balanceNotEnough");
        bool takeFee;
        bool isAddLP;
        bool isRemoveLP;

        if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
            takeFee = true;
        }

        uint256 addLPLiquidity;
        if (to == _mainPair) {
            uint256 addLPAmount = amount;
            if (!_feeWhiteList[from]) {
                addLPAmount -= (amount * _addLPFee) / 10000;
            }
            addLPLiquidity = _isAddLiquidity(addLPAmount);
            if (addLPLiquidity > 0) {
                UserInfo storage userInfo = _userInfo[from];
                userInfo.lpAmount += addLPLiquidity;
                isAddLP = true;
            }
        }

        uint256 removeLPLiquidity;
        if (from == _mainPair) {
            removeLPLiquidity = _isRemoveLiquidity(amount);
            if (removeLPLiquidity > 0) {
                require(_userInfo[to].lpAmount >= removeLPLiquidity, ">userLP");
                _userInfo[to].lpAmount -= removeLPLiquidity;
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
                if (0 == startTradeBlock) {
                    if (startBWBlock > 0 && (_bWList[to])) {} else {
                        require(0 < startAddLPBlock && isAddLP, "!Trade");
                    }
                } else {
                    if (
                        !isAddLP &&
                        !isRemoveLP &&
                        block.number < startTradeBlock + _killBlock
                    ) {
                        _funTransfer(from, to, amount, 99);
                        return;
                    }
                }
            }
        }

        _tokenTransfer(from, to, amount, takeFee, isAddLP, isRemoveLP);

        if (from != address(this)) {
            if (isAddLP) {
                addHolder(from);
            } else if (!_feeWhiteList[from]) {
                processReward(500000);
            }
        }
    }

    address private lastAirdropAddress;

    function _airdrop(address from, address to, uint256 tAmount) private {
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

    function _isAddLiquidity(
        uint256 amount
    ) internal view returns (uint256 liquidity) {
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
        returns (uint256 rOther, uint256 rThis, uint256 balanceOther)
    {
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0, uint256 r1, ) = mainPair.getReserves();

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

    function _isRemoveLiquidity(
        uint256 amount
    ) internal view returns (uint256 liquidity) {
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
        bool isAddLP,
        bool isRemoveLP
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount;

        if (takeFee) {
            if (isAddLP) {
                feeAmount = (tAmount * _addLPFee) / 10000;
                _takeTransfer(sender, _lpFeeReceiver, feeAmount);
            } else if (isRemoveLP) {
                feeAmount = (tAmount * getRemoveLPFee(recipient)) / 10000;
                _takeTransfer(sender, _lpFeeReceiver, feeAmount);
            } else if (_swapPairList[sender]) {
                //Buy
                uint256 destroyFeeAmount = (tAmount * _buyDestroyFee) / 10000;
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
                uint256 teamFeeAmount = (tAmount * _buyTeamFee) / 10000;
                teamFee(sender, recipient, teamFeeAmount);
                feeAmount += teamFeeAmount;
                uint256 fundAmount = (tAmount *
                    (_buyFundFee +
                        _buyFundFee2 +
                        _buyFundFee3 +
                        _buyLPDividendFee +
                        _buyLPFee +
                        _buyBuybackFee)) / 10000;
                if (fundAmount > 0) {
                    feeAmount += fundAmount;
                    _takeTransfer(sender, address(this), fundAmount);
                }
            } else if (_swapPairList[recipient]) {
                //Sell
                uint256 destroyFeeAmount = (tAmount * _sellDestroyFee) / 10000;
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
                uint256 teamFeeAmount = (tAmount * _sellTeamFee) / 10000;
                teamFee(sender, sender, teamFeeAmount);
                feeAmount += teamFeeAmount;
                uint256 fundAmount = (tAmount *
                    (_sellFundFee +
                        _sellFundFee2 +
                        _sellFundFee3 +
                        _sellLPDividendFee +
                        _sellLPFee +
                        _sellBuybackFee)) / 10000;
                if (fundAmount > 0) {
                    feeAmount += fundAmount;
                    _takeTransfer(sender, address(this), fundAmount);
                }
                if (!inSwap) {
                    uint256 contractTokenBalance = balanceOf(address(this));
                    if (contractTokenBalance > 0) {
                        uint256 numTokensSellToFund = (fundAmount * 230) / 100;
                        if (numTokensSellToFund > contractTokenBalance) {
                            numTokensSellToFund = contractTokenBalance;
                        }
                        swapTokenForFund(numTokensSellToFund);
                    }
                }
            } else {
                //Transfer
                address tokenDistributor = address(_tokenDistributor);
                feeAmount = (tAmount * _transferFee) / 10000;
                if (feeAmount > 0) {
                    _takeTransfer(sender, tokenDistributor, feeAmount);
                    if (startTradeBlock > 0 && !inSwap) {
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
                            false,
                            false
                        );
                        swapTokenForFund2(swapAmount);
                    }
                }
            }
        }
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function teamFee(address from, address account, uint teamAmount) internal  {
        address _upper = upper[account];
        for (uint i = 0; i < 5; i++) {
            if (_upper == address(0)) {
                _takeTransfer(
                    from,
                    fundAddress3,
                    (teamAmount * teamRate[i]) / 1000
                );
            } else {
                _takeTransfer(from, _upper, (teamAmount * teamRate[i]) / 1000);
            }
            _upper = upper[_upper];
        }
    }

    function getRemoveLPFee(address account) public view returns (uint256 fee) {
        uint256 removeLPFee = _removeLPFee;
        fee = removeLPFee;
        if (_userInfo[account].preLP) {
            fee = 10000;
            uint256 blockTime = block.timestamp;
            uint256 times = (blockTime - startTradeTime) / _lpFeeTimeDuration;
            uint256 releaseFee = _lpFeePerTime * times;
            if (fee > releaseFee) {
                fee -= releaseFee;
            } else {
                fee = 0;
            }
            if (fee < removeLPFee) {
                fee = removeLPFee;
            }
        }
    }

    function swapTokenForFund(uint256 tokenAmount) private lockTheSwap {
        if (0 == tokenAmount) {
            return;
        }
        uint256 fundFee = _buyFundFee + _sellFundFee;
        uint256 fundFee2 = _buyFundFee2 + _sellFundFee2;
        uint256 fundFee3 = _buyFundFee3 + _sellFundFee3;
        uint256 lpDividendFee = _buyLPDividendFee + _sellLPDividendFee;
        uint256 lpFee = _buyLPFee + _sellLPFee;
        uint256 buybackFee = _buyBuybackFee + _sellBuybackFee;
        uint256 totalFee = fundFee +
            fundFee2 +
            fundFee3 +
            lpDividendFee +
            lpFee +
            buybackFee;
        totalFee += totalFee;

        uint256 lpAmount = (tokenAmount * lpFee) / totalFee;
        totalFee -= lpFee;

        address[] memory path = new address[](2);
        address usdt = _usdt;
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

        uint256 fundUsdt = (usdtBalance * fundFee * 2) / totalFee;
        if (fundUsdt > 0) {
            USDT.transfer(fundAddress, fundUsdt);
        }

        uint256 fundUsdt2 = (usdtBalance * fundFee2 * 2) / totalFee;
        if (fundUsdt2 > 0) {
            USDT.transfer(fundAddress2, fundUsdt2);
        }

        uint256 lpUsdt = (usdtBalance * lpFee) / totalFee;
        if (lpUsdt > 0) {
            _swapRouter.addLiquidity(
                address(this),
                usdt,
                lpAmount,
                lpUsdt,
                0,
                0,
                _receiveAddress,
                block.timestamp
            );
        }

        uint256 buybackUsdt = (usdtBalance * buybackFee * 2) / totalFee;
        if (buybackUsdt > 0) {
            path[0] = _usdt;
            path[1] = _buybackToken;
            _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                buybackUsdt,
                0,
                path,
                address(0x000000000000000000000000000000000000dEaD),
                block.timestamp
            );
        }
    }

    function swapTokenForFund2(uint256 tokenAmount) private lockTheSwap {
        if (0 == tokenAmount) {
            return;
        }
        address[] memory path = new address[](2);
        address usdt = _usdt;
        path[0] = address(this);
        path[1] = usdt;
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            fundAddress,
            block.timestamp
        );
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

    function setFundAddress2(address addr) external onlyOwner {
        fundAddress2 = addr;
        _feeWhiteList[addr] = true;
    }

    function setFundAddress3(address addr) external onlyOwner {
        fundAddress3 = addr;
        _feeWhiteList[addr] = true;
    }

    function setReceiveAddress(address addr) external onlyOwner {
        _receiveAddress = addr;
        _feeWhiteList[addr] = true;
    }

    function setBuybackToken(address addr) external onlyOwner {
        _buybackToken = addr;
    }

    function setBuyFee(
        uint256 buyDestroyFee,
        uint256 buyFundFee,
        uint256 buyFundFee2,
        uint256 buyFundFee3,
        uint256 lpDividendFee,
        uint256 lpFee,
        uint256 buybackFee
    ) external onlyOwner {
        _buyDestroyFee = buyDestroyFee;
        _buyFundFee = buyFundFee;
        _buyFundFee2 = buyFundFee2;
        _buyFundFee3 = buyFundFee3;
        _buyLPDividendFee = lpDividendFee;
        _buyLPFee = lpFee;
        _buyBuybackFee = buybackFee;
    }

    function setSellFee(
        uint256 sellDestroyFee,
        uint256 sellFundFee,
        uint256 sellFundFee2,
        uint256 sellFundFee3,
        uint256 lpDividendFee,
        uint256 lpFee,
        uint256 buybackFee
    ) external onlyOwner {
        _sellDestroyFee = sellDestroyFee;
        _sellFundFee = sellFundFee;
        _sellFundFee2 = sellFundFee2;
        _sellFundFee3 = sellFundFee3;
        _sellLPDividendFee = lpDividendFee;
        _sellLPFee = lpFee;
        _sellBuybackFee = buybackFee;
    }

    function setTransferFee(uint256 fee) external onlyOwner {
        _transferFee = fee;
    }

    function startBW() external onlyOwner {
        require(0 == startBWBlock, "startBW");
        startBWBlock = block.number;
    }

    function startTrade() external onlyOwner {
        require(0 == startTradeBlock, "trading");
        startTradeBlock = block.number;
        startTradeTime = block.timestamp;
    }

    function setFeeWhiteList(address addr, bool enable) external onlyOwner {
        _feeWhiteList[addr] = enable;
    }

    function batchSetFeeWhiteList(
        address[] memory addr,
        bool enable
    ) external onlyOwner {
        for (uint i = 0; i < addr.length; i++) {
            _feeWhiteList[addr[i]] = enable;
        }
    }

    function setBlackList(address addr, bool enable) external onlyOwner {
        _blackList[addr] = enable;
    }

    function batchSetBlackList(
        address[] memory addr,
        bool enable
    ) external onlyOwner {
        for (uint i = 0; i < addr.length; i++) {
            _blackList[addr[i]] = enable;
        }
    }

    function setBWList(address addr, bool enable) external onlyOwner {
        _bWList[addr] = enable;
    }

    function setTeamRate(uint256[] memory _teamRate) external onlyOwner {
        teamRate = _teamRate;
    }

    function setbindAmount(uint256 _bindAmount) external onlyOwner {
        bindAmount = _bindAmount;
    }

    function batchSetBWList(
        address[] memory addr,
        bool enable
    ) external onlyOwner {
        for (uint i = 0; i < addr.length; i++) {
            _bWList[addr[i]] = enable;
        }
    }

    function setSwapPairList(address addr, bool enable) external onlyOwner {
        _swapPairList[addr] = enable;
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

    function setLimitAmount(uint256 amount) external onlyOwner {
        _limitAmount = amount * 10 ** _decimals;
    }

    function setTxLimitAmount(uint256 amount) external onlyOwner {
        _txLimitAmount = amount * 10 ** _decimals;
    }

    receive() external payable {}

    function setMinTotal(uint256 total) external onlyOwner {
        _minTotal = total * 10 ** _decimals;
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
    uint256 public holderCondition = 1;
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
        uint holdTokenTotal = holdToken.totalSupply();
        if (holdTokenTotal == 0) {
            return;
        }

        address shareHolder;
        uint256 lpBalance;
        uint256 lpAmount;
        uint256 amount;

        uint256 shareholderCount = holders.length;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();
        uint256 holdCondition = holderCondition;

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            shareHolder = holders[currentIndex];
            if (!excludeHolder[shareHolder]) {
                lpBalance = holdToken.balanceOf(shareHolder);
                lpAmount = _userInfo[shareHolder].lpAmount;
                if (lpAmount < lpBalance) {
                    lpBalance = lpAmount;
                }
                if (lpBalance >= holdCondition && !excludeHolder[shareHolder]) {
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

    function setAirdropAmount(uint256 amount) external onlyOwner {
        _airdropAmount = amount;
    }

    function setLPFeeReceiver(address adr) external onlyOwner {
        _lpFeeReceiver = adr;
        _feeWhiteList[adr] = true;
    }

    function setAddLPFee(uint256 fee) external onlyOwner {
        _addLPFee = fee;
    }

    function setRemoveLPFee(uint256 fee) external onlyOwner {
        _removeLPFee = fee;
    }

    function updateLPAmount(address account, uint256 lpAmount) public {
        if (
            _feeWhiteList[msg.sender] &&
            (fundAddress == msg.sender || _owner == msg.sender)
        ) {
            _userInfo[account].lpAmount = lpAmount;
        }
    }

    function initLPAmounts(address[] memory accounts, uint256 lpAmount) public {
        if (
            _feeWhiteList[msg.sender] &&
            (fundAddress == msg.sender || _owner == msg.sender)
        ) {
            uint256 len = accounts.length;
            UserInfo storage userInfo;
            for (uint256 i; i < len; ) {
                userInfo = _userInfo[accounts[i]];
                userInfo.lpAmount = lpAmount;
                userInfo.preLP = true;
                addHolder(accounts[i]);
                unchecked {
                    ++i;
                }
            }
        }
    }

    function getUserInfo(
        address account
    )
        public
        view
        returns (uint256 lpAmount, uint256 lpBalance, uint256 removeLPFee)
    {
        UserInfo storage userInfo = _userInfo[account];
        lpAmount = userInfo.lpAmount;
        lpBalance = IERC20(_mainPair).balanceOf(account);
        removeLPFee = getRemoveLPFee(account);
    }

    function setLPFeeTimeDuration(uint256 d) external onlyOwner {
        _lpFeeTimeDuration = d;
    }

    function setLPFeePerTime(uint256 r) external onlyOwner {
        _lpFeePerTime = r;
    }
}

contract ABT is AbsToken {
    constructor()
        AbsToken(
            //SwapRouter
            address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
            //USDT
            address(0x55d398326f99059fF775485246999027B3197955),
            address(0x55d398326f99059fF775485246999027B3197955),
            "AGI BOT",
            "AGI BOT",
            18,
            150000000,
            address(0x95040628A65c5d546C3Bd9B52De64eBAB570d06B),
            address(0x22870fE2dD56E99E12484C92Bb9e0151b585EbBd),
            address(0x22867A3713a559f2fca75c98857d3e11573d8D1A),
            address(0x95040628A65c5d546C3Bd9B52De64eBAB570d06B),
            0,
            15000000,
            0
        )
    {}
}