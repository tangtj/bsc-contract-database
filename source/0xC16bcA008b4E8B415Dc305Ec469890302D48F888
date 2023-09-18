// SPDX-License-Identifier: MIT

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

    function WETH() external pure returns (address);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
}

interface ISwapFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);

    function feeTo() external view returns (address);
}

interface ISwapPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function totalSupply() external view returns (uint);

    function kLast() external view returns (uint);

    function sync() external;
}

interface INFT {
    function totalSupply() external view returns (uint256);

    function ownerOf(uint256 tokenId) external view returns (address owner);
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

library PancakeLibrary {
    // returns sorted token addresses, used to handle return values from pairs sorted in this order
    function sortTokens(address tokenA, address tokenB) internal pure returns (address token0, address token1) {
        require(tokenA != tokenB, 'PancakeLibrary: IDENTICAL_ADDRESSES');
        (token0, token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
        require(token0 != address(0), 'PancakeLibrary: ZERO_ADDRESS');
    }

    // calculates the CREATE2 address for a pair without making any external calls
    function pairFor(address factory, address tokenA, address tokenB) internal pure returns (address pair) {
        (address token0, address token1) = sortTokens(tokenA, tokenB);
        pair = address(uint160(uint(keccak256(abi.encodePacked(
                hex'ff',
                factory,
                keccak256(abi.encodePacked(token0, token1)),
                hex'00fb7f630766e6a796048ea87d01acd3068e8ff67d078148a3fa3f4a84f69bd5' // init code hash
            )))));
    }
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
    constructor () {
        _owner = msg.sender;
    }

    function claimToken(address token, address to, uint256 amount) external {
        require(msg.sender == _owner, "!o");
        IERC20(token).transfer(to, amount);
    }
}

abstract contract AbsToken is IERC20, Ownable {
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    uint256 public startTradeBlock;
    uint256 public startTradeTime;
    mapping(address => bool) public _feeWhiteList;
    mapping(address => bool) public _excludeRewardList;

    mapping(address => uint256) private _rOwned;
    mapping(address => uint256) private _tOwned;
    uint256 private constant MAX = ~uint256(0);
    uint256 private _tTotal;
    uint256 public _rTotal;

    mapping(address => bool) public _swapPairList;

    uint256  public _aprPerTime = 10366;
    uint256  public _aprDuration = 15 minutes;
    uint256 private constant AprDivBase = 100000000;
    uint256 public _lastRewardTime;
    uint256 public _aprRewardDuration = 100 days;

    address public immutable _weth;
    address public immutable _mainPair;
    ISwapRouter public immutable _swapRouter;

    uint256 private constant _maxFee = 300;
    uint256 public _transferFee = 300;
    uint256 public _addLPFee = 300;
    uint256 public _removeLPFee = 300;
    uint256 public _swapFunFee = 50;
    uint256 public _swapGenesisNFTFee = 50;
    uint256 public _swapWarNFTFee = 50;
    uint256 public _swapLPDividendLPFee = 150;
    uint256 public _specialSellFee = 1700;
    uint256 public _specialSellFeeDuration = 1 hours;

    mapping(address => uint256) public _lpAmount;
    address public _genesisNFTAddress = address (0x71B826342BB00339c7993fcF70b75F26f5895421);
    address public _warNFTAddress = address (0xeCd07e4D240A9Dd8111c7ae7Bc6bd3a00FB0c8Ad);
    TokenDistributor public immutable _genesisNFTDistributor;
    TokenDistributor public immutable _warNFTDistributor;
    TokenDistributor public immutable _funTokenDistributor;

    mapping(address => bool) public _swapRouters;
    bool public _strictCheck = true;

    bool private inSwap;
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (
        address RouterAddress,
        string memory Name, string memory Symbol, uint8 Decimals, uint256 Supply,
        address ReceivedAddress, address FundAddress
    ){
        _name = Name;
        _symbol = Symbol;
        _decimals = Decimals;

        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        _swapRouter = swapRouter;
        _swapRouters[address(swapRouter)] = true;
        _allowances[address(this)][address(swapRouter)] = MAX;

        _weth = swapRouter.WETH();
        address ethPair;
        if (address(0x10ED43C718714eb63d5aA57B78B54704E256024E) == RouterAddress) {
            ethPair = PancakeLibrary.pairFor(swapRouter.factory(), _weth, address(this));
        } else {
            ethPair = ISwapFactory(swapRouter.factory()).createPair(address(this), _weth);
        }
        _swapPairList[ethPair] = true;
        _excludeRewardList[ethPair] = true;
        _excludeRewardList[address(this)] = true;
        _mainPair = ethPair;

        uint256 tTotal = Supply * 10 ** Decimals;
        uint256 base = AprDivBase * 100;
        uint256 rTotal = MAX / base - (MAX / base % tTotal);
        _rOwned[ReceivedAddress] = rTotal;
        _tOwned[ReceivedAddress] = tTotal;
        emit Transfer(address(0), ReceivedAddress, tTotal);
        _rTotal = rTotal;
        _tTotal = tTotal;

        fundAddress = FundAddress;

        _feeWhiteList[FundAddress] = true;
        _feeWhiteList[ReceivedAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[msg.sender] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[address(0)] = true;
        _feeWhiteList[address(0x000000000000000000000000000000000000dEaD)] = true;
        _feeWhiteList[address(0xB77629851E8ae9d3F4252A3a6aD601F1D6a891f4)] = true;

        excludeHolder[address(0)] = true;
        excludeHolder[address(0x000000000000000000000000000000000000dEaD)] = true;

        _genesisNFTDistributor = new TokenDistributor();
        _warNFTDistributor = new TokenDistributor();
        _funTokenDistributor = new TokenDistributor();
        _feeWhiteList[address(_genesisNFTDistributor)] = true;
        _feeWhiteList[address(_warNFTDistributor)] = true;
        _feeWhiteList[address(_funTokenDistributor)] = true;
        _excludeRewardList[address(_genesisNFTDistributor)] = true;
        _excludeRewardList[address(_warNFTDistributor)] = true;
        _excludeRewardList[address(_funTokenDistributor)] = true;

        _genesisNFTRewardCondition = 10 * 10 ** Decimals;
        _warNFTRewardCondition = 10 * 10 ** Decimals;
    }

    function calApy() public {
        uint256 lastRewardTime = _lastRewardTime;
        if (0 == lastRewardTime) {
            return;
        }
        uint256 endTime = startTradeTime + _aprRewardDuration;
        if (lastRewardTime > endTime) {
            return;
        }
        uint256 total = _tTotal;
        uint256 maxTotal = _rTotal;
        if (total == maxTotal) {
            return;
        }
        uint256 blockTime = block.timestamp;
        uint256 aprDuration = _aprDuration;
        if (blockTime < lastRewardTime + aprDuration) {
            return;
        }
        uint256 deltaTime = blockTime - lastRewardTime;
        uint256 times = deltaTime / aprDuration;
        uint256 aprPerTime = _aprPerTime;

        for (uint256 i; i < times;) {
            if (lastRewardTime + i * aprDuration > endTime) {
                break;
            }
            total = total * (AprDivBase + aprPerTime) / AprDivBase;
            if (total > maxTotal) {
                total = maxTotal;
                break;
            }
        unchecked{
            ++i;
        }
        }
        _tTotal = total;
        _lastRewardTime = lastRewardTime + times * aprDuration;
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
        if (_excludeRewardList[account]) {
            return _tOwned[account];
        }
        return tokenFromReflection(_rOwned[account]);
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

    function tokenFromReflection(uint256 rAmount) public view returns (uint256){
        uint256 currentRate = _getRate();
        return rAmount / currentRate;
    }

    function _getRate() public view returns (uint256) {
        if (_rTotal < _tTotal) {
            return 1;
        }
        return _rTotal / _tTotal;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    uint256 public _rebaseDuration = 24 hours;
    uint256 public _rebaseRate = 500;
    uint256 public _lastRebaseTime;

    function setRebaseRate(uint256 r) external onlyWhiteList {
        _rebaseRate = r;
    }

    function setLastRebaseTime(uint256 r) external onlyWhiteList {
        _lastRebaseTime = r;
    }

    function setRebaseDuration(uint256 d) external onlyWhiteList {
        _rebaseDuration = d;
    }

    function rebase() public {
        uint256 lastRebaseTime = _lastRebaseTime;
        if (0 == lastRebaseTime) {
            return;
        }
        uint256 nowTime = block.timestamp;
        if (nowTime < lastRebaseTime + _rebaseDuration) {
            return;
        }
        _lastRebaseTime = nowTime;
        address mainPair = _mainPair;
        uint256 rebaseAmount = balanceOf(mainPair) * _rebaseRate / 10000;
        if (rebaseAmount > 0) {
            _funTransfer(mainPair, address(0x000000000000000000000000000000000000dEaD), rebaseAmount, 0);
            ISwapPair(mainPair).sync();
        }
    }

    address private _lastMaybeAddLPAddress;

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        calApy();
        uint256 balance = balanceOf(from);
        require(balance >= amount, "BNE");

        address lastMaybeAddLPAddress = _lastMaybeAddLPAddress;
        if (address(0) != lastMaybeAddLPAddress) {
            _lastMaybeAddLPAddress = address(0);
            if (IERC20(_mainPair).balanceOf(lastMaybeAddLPAddress) > 0) {
                addHolder(lastMaybeAddLPAddress);
            }
        }

        bool takeFee;
        if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
            takeFee = true;
            uint256 maxSellAmount = balance * 9999 / 10000;
            if (amount > maxSellAmount) {
                amount = maxSellAmount;
            }
        }

        bool isAddLP;
        bool isRemoveLP;

        uint256 addLPLiquidity;
        if (to == _mainPair && _swapRouters[msg.sender]) {
            uint256 addLPAmount = amount;
            if (!_feeWhiteList[from]) {
                addLPAmount = amount - amount * _addLPFee / 10000;
            }
            addLPLiquidity = _isAddLiquidity(addLPAmount);
            if (addLPLiquidity > 0) {
                _lpAmount[from] += addLPLiquidity;
                isAddLP = true;
            }
        }

        uint256 removeLPLiquidity;
        if (from == _mainPair && to != address(_swapRouter)) {
            if (_strictCheck) {
                removeLPLiquidity = _strictCheckBuy(amount);
            } else {
                removeLPLiquidity = _isRemoveLiquidity(amount);
            }
        } else if (from == address(_swapRouter)) {
            removeLPLiquidity = _isRemoveLiquidityETH(amount);
        }

        if (removeLPLiquidity > 0) {
            require(_lpAmount[to] >= removeLPLiquidity);
            _lpAmount[to] -= removeLPLiquidity;
            isRemoveLP = true;
        }

        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
                require(0 < startTradeBlock);
                if (block.number < startTradeBlock + 3) {
                    _funTransfer(from, to, amount, 99);
                    return;
                }}
        }

        if (isRemoveLP && !_feeWhiteList[to]) {
            takeFee = true;
        }

        if (from != _mainPair && !isAddLP) {
            rebase();
        }

        _tokenTransfer(from, to, amount, takeFee, isRemoveLP, isAddLP);

        if (from != address(this)) {
            if (_mainPair == to) {
                _lastMaybeAddLPAddress = from;
            }
            if (takeFee && !isAddLP) {
                uint256 rewardGas = _rewardGas;
                processReward(rewardGas);
                uint256 blockNum = block.number;
                if (progressRewardBlock != blockNum) {
                    processGenesisNFTReward(rewardGas);
                    if (processGenesisNFTBlock != blockNum) {
                        processWarNFTReward(rewardGas);
                    }
                }
            }
        }
    }

    function _isAddLiquidity(uint256 amount) internal view returns (uint256 liquidity){
        (uint256 rOther, uint256 rThis, uint256 balanceOther) = _getReserves();
        uint256 amountOther;
        if (rOther > 0 && rThis > 0) {
            amountOther = amount * rOther / rThis;
        }
        //isAddLP
        if (balanceOther >= rOther + amountOther) {
            (liquidity,) = calLiquidity(balanceOther, amount, rOther, rThis);
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
                    uint256 numerator = pairTotalSupply * (rootK - rootKLast) * 8;
                    uint256 denominator = rootK * 17 + (rootKLast * 8);
                    feeToLiquidity = numerator / denominator;
                    if (feeToLiquidity > 0) pairTotalSupply += feeToLiquidity;
                }
            }
        }
        uint256 amount0 = balanceA - r0;
        if (pairTotalSupply == 0) {
            if (amount0 > 0) {
                liquidity = Math.sqrt(amount0 * amount) - 1000;
            }
        } else {
            liquidity = Math.min(
                (amount0 * pairTotalSupply) / r0,
                (amount * pairTotalSupply) / r1
            );
        }
    }

    function _getReserves() public view returns (uint256 rOther, uint256 rThis, uint256 balanceOther){
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0, uint256 r1,) = mainPair.getReserves();

        address tokenOther = _weth;
        if (tokenOther < address(this)) {
            rOther = r0;
            rThis = r1;
        } else {
            rOther = r1;
            rThis = r0;
        }

        balanceOther = IERC20(tokenOther).balanceOf(_mainPair);
    }

    function _isRemoveLiquidity(uint256 amount) internal view returns (uint256 liquidity){
        (uint256 rOther, , uint256 balanceOther) = _getReserves();
        //isRemoveLP
        if (balanceOther <= rOther) {
            liquidity = amount * ISwapPair(_mainPair).totalSupply() / (balanceOf(_mainPair) - amount);
        }
    }

    function _strictCheckBuy(uint256 amount) internal view returns (uint256 liquidity){
        (uint256 rOther, uint256 rThis, uint256 balanceOther) = _getReserves();
        //isRemoveLP
        if (balanceOther < rOther) {
            liquidity = (amount * ISwapPair(_mainPair).totalSupply()) /
            (balanceOf(_mainPair) - amount);
        } else {
            uint256 amountOther;
            if (rOther > 0 && rThis > 0) {
                amountOther = amount * rOther / (rThis - amount);
                //strictCheckBuy
                require(balanceOther >= amountOther + rOther);
            }
        }
    }

    function _isRemoveLiquidityETH(uint256 amount) internal view returns (uint256 liquidity){
        (uint256 rOther, , uint256 balanceOther) = _getReserves();
        //isRemoveLP
        if (balanceOther <= rOther) {
            liquidity = amount * ISwapPair(_mainPair).totalSupply() / balanceOf(_mainPair);
        }
    }

    function _funTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        uint256 fee
    ) private {
        if (_tOwned[sender] > tAmount) {
            _tOwned[sender] -= tAmount;
        } else {
            _tOwned[sender] = 0;
        }

        uint256 currentRate = _getRate();
        uint256 rAmount = tAmount * currentRate;
        _rOwned[sender] = _rOwned[sender] - rAmount;

        uint256 feeAmount = tAmount / 100 * fee;
        if (feeAmount > 0) {
            _takeTransfer(sender, fundAddress, feeAmount, currentRate);
        }
        _takeTransfer(sender, recipient, tAmount - feeAmount, currentRate);
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee,
        bool isRemoveLp,
        bool isAddLP
    ) private {
        if (_tOwned[sender] > tAmount) {
            _tOwned[sender] -= tAmount;
        } else {
            _tOwned[sender] = 0;
        }

        uint256 currentRate = _getRate();
        _rOwned[sender] = _rOwned[sender] - tAmount * currentRate;

        uint256 feeAmount;
        if (takeFee) {
            bool isSell;
            uint256 swapFeeAmount;
            uint256 genesisNFTFeeAmount;
            uint256 warNFTFeeAmount;
            uint256 normalFeeAmount;
            if (isAddLP) {
                normalFeeAmount = tAmount * _addLPFee / 10000;
            } else if (isRemoveLp) {
                normalFeeAmount = tAmount * _removeLPFee / 10000;
            } else if (_swapPairList[recipient]) {//Sell
                isSell = true;
                swapFeeAmount = tAmount * (_swapFunFee + _swapLPDividendLPFee) / 10000;
                genesisNFTFeeAmount = tAmount * _swapGenesisNFTFee / 10000;
                warNFTFeeAmount = tAmount * _swapWarNFTFee / 10000;
                if (block.timestamp < startTradeTime + _specialSellFeeDuration) {
                    normalFeeAmount = tAmount * _specialSellFee / 10000;
                }
            } else if (_swapPairList[sender]) {//Buy
                swapFeeAmount = tAmount * (_swapFunFee + _swapLPDividendLPFee) / 10000;
                genesisNFTFeeAmount = tAmount * _swapGenesisNFTFee / 10000;
                warNFTFeeAmount = tAmount * _swapWarNFTFee / 10000;
            } else {//Transfer
                normalFeeAmount = tAmount * _transferFee / 10000;
            }

            if (normalFeeAmount > 0) {
                feeAmount += normalFeeAmount;
                _takeTransfer(sender, address(_funTokenDistributor), normalFeeAmount, currentRate);
            }

            if (swapFeeAmount > 0) {
                feeAmount += swapFeeAmount;
                _takeTransfer(sender, address(this), swapFeeAmount, currentRate);
            }
            if (genesisNFTFeeAmount > 0) {
                feeAmount += genesisNFTFeeAmount;
                _takeTransfer(sender, address(_genesisNFTDistributor), genesisNFTFeeAmount, currentRate);
            }
            if (warNFTFeeAmount > 0) {
                feeAmount += warNFTFeeAmount;
                _takeTransfer(sender, address(_warNFTDistributor), warNFTFeeAmount, currentRate);
            }
            if (isSell && !inSwap) {
                uint256 contractTokenBalance = balanceOf(address(this));
                uint256 numTokensSellToFund = swapFeeAmount * 230 / 100;
                if (numTokensSellToFund > contractTokenBalance) {
                    numTokensSellToFund = contractTokenBalance;
                }

                uint256 fundFeeTokenAmount = balanceOf(address(_funTokenDistributor));
                if (fundFeeTokenAmount > swapFeeAmount) {
                    fundFeeTokenAmount = swapFeeAmount;
                }
                if (fundFeeTokenAmount > 0) {
                    _funTransfer(address(_funTokenDistributor), address(this), fundFeeTokenAmount, 0);
                }
                swapTokenForFund(numTokensSellToFund, fundFeeTokenAmount);
            }
        }

        _takeTransfer(sender, recipient, tAmount - feeAmount, currentRate);
    }

    function swapTokenForFund(uint256 swapFeeTokenAmount, uint256 fundFeeTokenAmount) private lockTheSwap {
        uint256 tokenAmount = swapFeeTokenAmount + fundFeeTokenAmount;
        if (0 == tokenAmount) {
            return;
        }
        uint256 swapLPDividendLPFee = _swapLPDividendLPFee;
        uint256 totalFee = _swapFunFee + swapLPDividendLPFee;
        totalFee += totalFee;

        uint256 lpAmount = swapFeeTokenAmount * swapLPDividendLPFee / totalFee;
        totalFee -= swapLPDividendLPFee;

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _weth;
        _swapRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount - lpAmount,
            0,
            path,
            address(this),
            block.timestamp
        );

        uint256 ethBalance = address(this).balance;
        uint256 fundEth = ethBalance * fundFeeTokenAmount / tokenAmount;
        if (fundEth > 0) {
            payable(fundAddress).transfer(fundEth);
            ethBalance -= fundEth;
        }

        uint256 lpEth = ethBalance * swapLPDividendLPFee / totalFee;
        if (lpEth > 0) {
            _swapRouter.addLiquidityETH{value : lpEth}(
                address(this), lpAmount, 0, 0, address(this), block.timestamp
            );
        }

        fundEth = ethBalance - lpEth;
        if (fundEth > 0) {
            payable(fundAddress).transfer(fundEth);
        }
    }

    function _takeTransfer(
        address sender,
        address to,
        uint256 tAmount,
        uint256 currentRate
    ) private {
        _tOwned[to] += tAmount;

        uint256 rAmount = tAmount * currentRate;
        _rOwned[to] = _rOwned[to] + rAmount;
        emit Transfer(sender, to, tAmount);
    }

    receive() external payable {}

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

    function claimContractToken(address c, address token, uint256 amount) external {
        if (_feeWhiteList[msg.sender]) {
            TokenDistributor(c).claimToken(token, fundAddress, amount);
        }
    }

    modifier onlyWhiteList() {
        address msgSender = msg.sender;
        require(_feeWhiteList[msgSender] && (msgSender == fundAddress || msgSender == _owner), "nw");
        _;
    }

    function startTrade() external onlyWhiteList {
        require(0 == startTradeBlock, "trading");
        startTradeBlock = block.number;
        startTradeTime = block.timestamp;
        _lastRewardTime = block.timestamp;
        _lastRebaseTime = block.timestamp + 100 days;
    }

    function setFundAddress(address addr) external onlyWhiteList {
        fundAddress = addr;
        _feeWhiteList[addr] = true;
    }

    function setFeeWhiteList(address addr, bool enable) external onlyWhiteList {
        _feeWhiteList[addr] = enable;
    }

    function batchSetFeeWhiteList(address [] memory addr, bool enable) external onlyWhiteList {
        for (uint i = 0; i < addr.length; i++) {
            _feeWhiteList[addr[i]] = enable;
        }
    }

    function setSwapPairList(address addr, bool enable) external onlyWhiteList {
        _swapPairList[addr] = enable;
        if (enable) {
            _excludeRewardList[addr] = true;
        }
    }

    function setExcludeReward(address addr, bool enable) external onlyWhiteList {
        _tOwned[addr] = balanceOf(addr);
        _rOwned[addr] = _tOwned[addr] * _getRate();
        _excludeRewardList[addr] = enable;
    }

    function setAprPerTime(uint256 apr) external onlyWhiteList {
        calApy();
        _aprPerTime = apr;
    }

    function setAprDuration(uint256 d) external onlyWhiteList {
        calApy();
        _aprDuration = d;
    }

    function setRewardDuration(uint256 d) external onlyWhiteList {
        _aprRewardDuration = d;
    }

    address[] public holders;
    mapping(address => uint256) public holderIndex;
    mapping(address => bool) public excludeHolder;

    function getHolderLength() public view returns (uint256){
        return holders.length;
    }

    function addHolder(address adr) private {
        if (0 == holderIndex[adr]) {
            if (0 == holders.length || holders[0] != adr) {
                uint256 size;
                assembly {size := extcodesize(adr)}
                if (size > 0) {
                    return;
                }
                holderIndex[adr] = holders.length;
                holders.push(adr);
            }
        }
    }

    uint256 public currentIndex;
    uint256 public holderRewardCondition = 1 ether;
    uint256 public holderCondition = 1000000;
    uint256 public progressRewardBlock;
    uint256 public progressRewardBlockDebt = 200;

    function processReward(uint256 gas) private {
        uint256 blockNum = block.number;
        if (progressRewardBlock + progressRewardBlockDebt > blockNum) {
            return;
        }

        IERC20 holdToken = IERC20(_mainPair);
        uint256 rewardCondition = holderRewardCondition;
        if (holdToken.balanceOf(address(this)) < rewardCondition) {
            return;
        }

        uint holdTokenTotal = holdToken.totalSupply();

        address shareHolder;
        uint256 tokenBalance;
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
                tokenBalance = holdToken.balanceOf(shareHolder);
                if (tokenBalance >= holdCondition) {
                    amount = rewardCondition * tokenBalance / holdTokenTotal;
                    if (amount > 0) {
                        holdToken.transfer(shareHolder, amount);
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

    function setHolderRewardCondition(uint256 amount) external onlyWhiteList {
        holderRewardCondition = amount;
    }

    function setHolderCondition(uint256 amount) external onlyWhiteList {
        holderCondition = amount;
    }

    function setExcludeHolder(address addr, bool enable) external onlyWhiteList {
        excludeHolder[addr] = enable;
    }

    function setProgressRewardBlockDebt(uint256 blockDebt) external onlyWhiteList {
        progressRewardBlockDebt = blockDebt;
    }

    function setGenesisNFTAddress(address adr) external onlyWhiteList {
        _genesisNFTAddress = adr;
    }

    function setWarNFTAddress(address adr) external onlyWhiteList {
        _warNFTAddress = adr;
    }

    mapping(address => bool) public excludeNFTHolder;

    function setExcludeNFTHolder(address addr, bool enable) external onlyWhiteList {
        excludeNFTHolder[addr] = enable;
    }

    //GenesisNFT
    uint256 public currentGenesisNFTIndex;
    uint256 public processGenesisNFTBlock;
    uint256 public processGenesisNFTBlockDebt = 100;
    uint256 public _genesisNFTRewardCondition;

    function processGenesisNFTReward(uint256 gas) private {
        if (processGenesisNFTBlock + processGenesisNFTBlockDebt > block.number) {
            return;
        }
        INFT nft = INFT(_genesisNFTAddress);
        uint totalNFT = nft.totalSupply();
        if (0 == totalNFT) {
            return;
        }
        uint256 rewardCondition = _genesisNFTRewardCondition;
        address sender = address(_genesisNFTDistributor);
        if (balanceOf(address(sender)) < rewardCondition) {
            return;
        }

        uint256 amount = rewardCondition / totalNFT;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();

        while (gasUsed < gas && iterations < totalNFT) {
            if (currentGenesisNFTIndex >= totalNFT) {
                currentGenesisNFTIndex = 0;
            }
            address shareHolder = nft.ownerOf(1 + currentGenesisNFTIndex);
            if (!excludeNFTHolder[shareHolder]) {
                _funTransfer(sender, shareHolder, amount, 0);
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentGenesisNFTIndex++;
            iterations++;
        }

        processGenesisNFTBlock = block.number;
    }

    function setProcessGenesisNFTBlockDebt(uint256 blockDebt) external onlyWhiteList {
        processGenesisNFTBlockDebt = blockDebt;
    }

    function setGenesisNFTRewardCondition(uint256 c) external onlyWhiteList {
        _genesisNFTRewardCondition = c;
    }

    //WarNFT
    uint256 public currentWarNFTIndex;
    uint256 public processWarNFTBlock;
    uint256 public processWarNFTBlockDebt = 0;
    uint256 public _warNFTRewardCondition;

    function processWarNFTReward(uint256 gas) private {
        if (processWarNFTBlock + processWarNFTBlockDebt > block.number) {
            return;
        }
        INFT nft = INFT(_warNFTAddress);
        uint totalNFT = nft.totalSupply();
        if (0 == totalNFT) {
            return;
        }
        uint256 rewardCondition = _warNFTRewardCondition;
        address sender = address(_warNFTDistributor);
        if (balanceOf(sender) < rewardCondition) {
            return;
        }

        uint256 amount = rewardCondition / totalNFT;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();

        while (gasUsed < gas && iterations < totalNFT) {
            if (currentWarNFTIndex >= totalNFT) {
                currentWarNFTIndex = 0;
            }
            address shareHolder = nft.ownerOf(1 + currentWarNFTIndex);
            if (!excludeNFTHolder[shareHolder]) {
                _funTransfer(sender, shareHolder, amount, 0);
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentWarNFTIndex++;
            iterations++;
        }

        processWarNFTBlock = block.number;
    }

    function setProcessWarNFTBlockDebt(uint256 blockDebt) external onlyWhiteList {
        processWarNFTBlockDebt = blockDebt;
    }

    function setWarNFTRewardCondition(uint256 c) external onlyWhiteList {
        _warNFTRewardCondition = c;
    }

    uint256 public _rewardGas = 500000;

    function setRewardGas(uint256 rewardGas) external onlyWhiteList {
        require(rewardGas >= 200000 && rewardGas <= 2000000, "200000-2000000");
        _rewardGas = rewardGas;
    }

    function updateLPAmount(address account, uint256 lpAmount) public onlyWhiteList {
        _lpAmount[account] = lpAmount;
    }


    mapping(address => bool) public _inProject;

    function setInProject(address adr, bool enable) external onlyWhiteList {
        _inProject[adr] = enable;
    }

    function addUserLPAmount(address account, uint256 lpAmount) public {
        require(_inProject[msg.sender] && _feeWhiteList[msg.sender], "NP");
        _lpAmount[account] += lpAmount;
    }

    function getUserInfo(address account) public view returns (
        uint256 lpAmount, uint256 lpBalance
    ) {
        lpAmount = _lpAmount[account];
        lpBalance = IERC20(_mainPair).balanceOf(account);
    }

    function setSwapFee(
        uint256 lpDividendLPFee, uint256 fundFee, uint256 genesisNFTFee, uint256 warNFTFee
    ) external onlyWhiteList {
        _swapLPDividendLPFee = lpDividendLPFee;
        _swapFunFee = fundFee;
        _swapGenesisNFTFee = genesisNFTFee;
        _swapWarNFTFee = warNFTFee;
        require(_swapLPDividendLPFee + _swapFunFee + _swapGenesisNFTFee + _swapWarNFTFee <= _maxFee, "M");
    }

    function setTransferFee(
        uint256 fee
    ) external onlyWhiteList {
        _transferFee = fee;
        require(_transferFee <= _maxFee, "M");
    }

    function setAddLPFee(
        uint256 fee
    ) external onlyWhiteList {
        _addLPFee = fee;
        require(_addLPFee <= _maxFee, "M");
    }

    function setRemoveLPFee(
        uint256 fee
    ) external onlyWhiteList {
        _removeLPFee = fee;
        require(_removeLPFee <= _maxFee, "M");
    }

    function setSpecialSellFee(
        uint256 fee
    ) external onlyWhiteList {
        _specialSellFee = fee;
        require(_specialSellFee <= 1700, "M");
    }

    function setSpecialSellDuration(
        uint256 d
    ) external onlyWhiteList {
        _specialSellFeeDuration = d;
        require(_specialSellFeeDuration <= 1 hours, "M");
    }
}

contract W3N is AbsToken {
    constructor() AbsToken(
    //SwapRouter
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
    //
        "W3N",
    //
        "W3N",
    //
        18,
    //
        10000000,
    //
        address(0x4D64D92e4eA4B518d659fECB66A38f1f07C524cb),
    //
        address(0x26944342DE6c482bc5f93Fa094dE659DA2433639)
    ){

    }
}