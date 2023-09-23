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

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

interface ISwapFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);

    function feeTo() external view returns (address);
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

interface ISwapPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function totalSupply() external view returns (uint);

    function kLast() external view returns (uint);

    function sync() external;
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

abstract contract AbsToken is IERC20, Ownable {
    struct UserInfo {
        uint256 lpAmount;
        uint256 lock7DLPAmount;
        uint256 lock183DLPAmount;
    }

    mapping(address => uint256) public _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;
    address public fundAddressB;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _feeWhiteList;

    mapping(address => UserInfo) private _userInfo;

    uint256 private _tTotal;

    ISwapRouter public immutable _swapRouter;
    mapping(address => bool) public _swapPairList;
    mapping(address => bool) public _swapRouters;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public immutable _tokenDistributor;
    TokenDistributor public immutable _lpMintDistributor;

    uint256 private constant _buyLPDividendFee = 150;
    uint256  private constant _buyFundFee = 50;
    uint256  private constant _buyFundFeeB = 100;
    uint256  private constant _buyAirdropFee = 10;

    uint256 private constant _sellLPDividendFee = 150;
    uint256  private constant _sellFundFee = 50;
    uint256  private constant _sellFundFeeB = 200;

    uint256 private constant _preFeeDuration = 1 hours;
    uint256 private constant _preBuyFundFeeB = 290;
    uint256 private constant _preSellFundFeeB = 200;

    uint256 private constant _removeLPDividendFee = 100;
    uint256  private constant _removeFundFee = 100;
    uint256 private constant _lock7D = 7 days;
    uint256 private constant _lock183D = 183 days;

    uint256 public startTradeBlock;
    uint256 public startAddLPBlock;
    address public immutable _mainPair;
    address public  immutable _usdt;

    uint256 public _startTradeTime;

    bool public _strictCheck = true;
    uint256 public  immutable _minTotal;
    address public _ltc;

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (
        address RouterAddress, address UsdtAddress, address LTCAddress,
        string memory Name, string memory Symbol, uint8 Decimals, uint256 Supply,
        address ReceiveAddress, address FundAddress, address FundAddressB, uint256 MinTotal
    ){
        _name = Name;
        _symbol = Symbol;
        _decimals = Decimals;

        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;
        _swapRouters[address(swapRouter)] = true;
        _ltc = LTCAddress;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        _usdt = UsdtAddress;
        IERC20(_usdt).approve(address(swapRouter), MAX);

        address usdtPair;
        if (address(0x10ED43C718714eb63d5aA57B78B54704E256024E) == RouterAddress) {
            usdtPair = PancakeLibrary.pairFor(address(swapFactory), _usdt, address(this));
        } else {
            usdtPair = swapFactory.createPair(_usdt, address(this));
        }
        _swapPairList[usdtPair] = true;
        _mainPair = usdtPair;

        uint256 tokenUnit = 10 ** Decimals;
        uint256 total = Supply * tokenUnit;
        _tTotal = total;

        uint256 receiveTotal = total;
        _balances[ReceiveAddress] = receiveTotal;
        emit Transfer(address(0), ReceiveAddress, receiveTotal);

        fundAddress = FundAddress;
        fundAddressB = FundAddressB;
        _tokenDistributor = new TokenDistributor(UsdtAddress);
        _lpMintDistributor = new TokenDistributor(UsdtAddress);

        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[FundAddress] = true;
        _feeWhiteList[FundAddressB] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;
        _feeWhiteList[address(0)] = true;
        _feeWhiteList[address(0x000000000000000000000000000000000000dEaD)] = true;
        _feeWhiteList[address(_tokenDistributor)] = true;
        _feeWhiteList[address(_lpMintDistributor)] = true;

        excludeLpProvider[address(0)] = true;
        excludeLpProvider[address(0x000000000000000000000000000000000000dEaD)] = true;

        _minTotal = MinTotal * tokenUnit;
        _mintRewardCondition = 65 * tokenUnit;
        _rebaseAmount = 130 * tokenUnit;
        lpRewardCondition = 10 ** IERC20(LTCAddress).decimals();
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

    function validSupply() public view returns (uint256) {
        return _tTotal - _balances[address(0)] - _balances[address(0x000000000000000000000000000000000000dEaD)];
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
        require(balance >= amount, "BNE");

        bool takeFee;
        if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
            uint256 maxSellAmount = balance * 999999 / 1000000;
            if (amount > maxSellAmount) {
                amount = maxSellAmount;
            }
            takeFee = true;
        }

        bool isAddLP;
        bool isRemoveLP;
        UserInfo storage userInfo;

        uint256 addLPLiquidity;
        if (to == _mainPair && _swapRouters[msg.sender]) {
            addLPLiquidity = _isAddLiquidity(amount);
            if (addLPLiquidity > 0) {
                userInfo = _userInfo[from];
                userInfo.lpAmount += addLPLiquidity;
                isAddLP = true;
                takeFee = false;
                if (0 == _lastLPMintRewardTimes[from]) {
                    _lastLPMintRewardTimes[from] = block.timestamp;
                }
            }
        }

        uint256 removeLPLiquidity;
        if (from == _mainPair) {
            if (_strictCheck) {
                removeLPLiquidity = _strictCheckBuy(amount);
            } else {
                removeLPLiquidity = _isRemoveLiquidity(amount);
            }
            if (removeLPLiquidity > 0) {
                require(_userInfo[to].lpAmount >= removeLPLiquidity);
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
                    require(0 < startAddLPBlock && isAddLP);
                } else if (block.number < startTradeBlock + 2) {
                    _funTransfer(from, to, amount, 99);
                    return;
                }
            }
        }

        if (from != _mainPair && !isAddLP) {
            rebase();
        }

        _tokenTransfer(from, to, amount, takeFee, removeLPLiquidity);

        if (from != address(this)) {
            if (isAddLP) {
                _addLpProvider(from);
            } else if (takeFee) {
                uint256 rewardGas = _rewardGas;
                processLPReward(rewardGas);
                if (block.number != progressLPRewardBlock) {
                    processLPMintReward(rewardGas);
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

    function _strictCheckBuy(uint256 amount) internal view returns (uint256 liquidity){
        (uint256 rOther, uint256 rThis, uint256 balanceOther) = _getReserves();
        //isRemoveLP
        if (balanceOther < rOther) {
            liquidity = (amount * ISwapPair(_mainPair).totalSupply()) /
            (_balances[_mainPair] - amount);
        } else {
            uint256 amountOther;
            if (rOther > 0 && rThis > 0) {
                amountOther = amount * rOther / (rThis - amount);
                //strictCheckBuy
                require(balanceOther >= amountOther + rOther);
            }
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
                    uint256 numerator;
                    uint256 denominator;
                    if (address(_swapRouter) == address(0x10ED43C718714eb63d5aA57B78B54704E256024E)) {// BSC Pancake
                        numerator = pairTotalSupply * (rootK - rootKLast) * 8;
                        denominator = rootK * 17 + (rootKLast * 8);
                    } else if (address(_swapRouter) == address(0xD99D1c33F9fC3444f8101754aBC46c52416550D1)) {//BSC testnet Pancake
                        numerator = pairTotalSupply * (rootK - rootKLast);
                        denominator = rootK * 3 + rootKLast;
                    } else if (address(_swapRouter) == address(0xE9d6f80028671279a28790bb4007B10B0595Def1)) {//PG W3Swap
                        numerator = pairTotalSupply * (rootK - rootKLast) * 3;
                        denominator = rootK * 5 + rootKLast;
                    } else {//SushiSwap,UniSwap,OK Cherry Swap
                        numerator = pairTotalSupply * (rootK - rootKLast);
                        denominator = rootK * 5 + rootKLast;
                    }
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

    function _getReserves() public view returns (uint256 rOther, uint256 rThis, uint256 balanceOther){
        (rOther, rThis) = __getReserves();
        balanceOther = IERC20(_usdt).balanceOf(_mainPair);
    }

    function __getReserves() public view returns (uint256 rOther, uint256 rThis){
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0, uint256 r1,) = mainPair.getReserves();

        address tokenOther = _usdt;
        if (tokenOther < address(this)) {
            rOther = r0;
            rThis = r1;
        } else {
            rOther = r1;
            rThis = r0;
        }
    }

    function _isRemoveLiquidity(uint256 amount) internal view returns (uint256 liquidity){
        (uint256 rOther, , uint256 balanceOther) = _getReserves();
        //isRemoveLP
        if (balanceOther < rOther) {
            liquidity = (amount * ISwapPair(_mainPair).totalSupply()) /
            (_balances[_mainPair] - amount);
        }
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
            _takeTransfer(sender, address(0x000000000000000000000000000000000000dEaD), feeAmount);
        }
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    address private lastAirdropAddress;

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee,
        uint256 removeLPLiquidity
    ) private {
        uint256 senderBalance = _balances[sender];
        senderBalance -= tAmount;
        _balances[sender] = senderBalance;
        uint256 feeAmount;

        if (takeFee) {
            bool isSell;
            uint256 swapFeeAmount;
            bool isPreFee;
            if (removeLPLiquidity > 0) {
                feeAmount += _calRemoveFeeAmount(sender, recipient, tAmount, removeLPLiquidity);
            } else if (_swapPairList[sender]) {//Buy
                swapFeeAmount = tAmount * (_buyLPDividendFee + _buyFundFee + _buyFundFeeB) / 10000;
                isPreFee = block.timestamp < _startTradeTime + _preFeeDuration;
                if (isPreFee) {
                    swapFeeAmount += tAmount * _preBuyFundFeeB / 10000;
                }
                uint256 airdropFeeAmount = _buyAirdropFee * tAmount / 10000;
                if (airdropFeeAmount > 0) {
                    uint256 seed = (uint160(lastAirdropAddress) | block.number) ^ uint160(recipient);
                    feeAmount += airdropFeeAmount;
                    uint256 airdropAmount = airdropFeeAmount / 2;
                    address airdropAddress;
                    for (uint256 i; i < 2;) {
                        airdropAddress = address(uint160(seed | tAmount));
                        _takeTransfer(sender, airdropAddress, airdropAmount);
                    unchecked{
                        ++i;
                        seed = seed >> 1;
                    }
                    }
                    lastAirdropAddress = airdropAddress;
                }
            } else if (_swapPairList[recipient]) {//Sell
                isSell = true;
                swapFeeAmount = tAmount * (_sellLPDividendFee + _sellFundFee + _sellFundFeeB) / 10000;
                isPreFee = block.timestamp < _startTradeTime + _preFeeDuration;
                if (isPreFee) {
                    swapFeeAmount += tAmount * _preSellFundFeeB / 10000;
                }
            }

            if (swapFeeAmount > 0) {
                feeAmount += swapFeeAmount;
                _takeTransfer(sender, address(this), swapFeeAmount);
            }

            if (isSell && !inSwap) {
                uint256 contractTokenBalance = _balances[address(this)];
                uint256 numToSell = swapFeeAmount * 230 / 100;
                if (numToSell > contractTokenBalance) {
                    numToSell = contractTokenBalance;
                }

                uint256 removeNumToSell = swapFeeAmount;
                uint256 distributorTokenBalance = _balances[address(_tokenDistributor)];
                if (removeNumToSell > distributorTokenBalance) {
                    removeNumToSell = distributorTokenBalance;
                }
                if (removeNumToSell > 0) {
                    _funTransfer(address(_tokenDistributor), address(this), removeNumToSell, 0);
                }

                swapTokenForFund(numToSell + removeNumToSell, removeNumToSell, isPreFee);
            }
        }

        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function _calRemoveFeeAmount(address sender, address recipient, uint256 tAmount, uint256 removeLPLiquidity) private returns (uint256 feeAmount){
        UserInfo storage userInfo = _userInfo[recipient];
        uint256 startTime = _startTradeTime;
        uint256 nowTime = block.timestamp;
        //check Lock
        if (nowTime < startTime + _lock7D) {
            require(userInfo.lpAmount >= userInfo.lock7DLPAmount + userInfo.lock183DLPAmount);
        } else if (nowTime < startTime + _lock183D) {
            require(userInfo.lpAmount >= userInfo.lock183DLPAmount);
        }
        //自己加的LP
        uint256 selfLPAmount = userInfo.lpAmount + removeLPLiquidity - userInfo.lock7DLPAmount - userInfo.lock183DLPAmount;
        uint256 removeLockLPAmount = removeLPLiquidity;
        uint256 removeSelfLPAmount = removeLPLiquidity;
        if (removeLPLiquidity > selfLPAmount) {
            removeSelfLPAmount = selfLPAmount;
        }
        if (removeSelfLPAmount > 0) {
            removeLockLPAmount -= removeSelfLPAmount;
            uint256 selfLPFeeAmount = tAmount * removeSelfLPAmount / removeLPLiquidity * (_removeLPDividendFee + _removeFundFee) / 10000;
            feeAmount += selfLPFeeAmount;
            _takeTransfer(sender, address(_tokenDistributor), selfLPFeeAmount);
        }
        uint256 destroyFeeAmount = tAmount * removeLockLPAmount / removeLPLiquidity;
        if (destroyFeeAmount > 0) {
            feeAmount += destroyFeeAmount;
            _takeTransfer(sender, address(0x000000000000000000000000000000000000dEaD), destroyFeeAmount);
        }
        if (userInfo.lock7DLPAmount > removeLockLPAmount) {
            userInfo.lock7DLPAmount -= removeLockLPAmount;
        } else {
            removeLockLPAmount -= userInfo.lock7DLPAmount;
            userInfo.lock7DLPAmount = 0;
            userInfo.lock183DLPAmount -= removeLockLPAmount;
        }
    }

    function swapTokenForFund(uint256 tokenAmount, uint256 removeNumToSell, bool isPreFee) private lockTheSwap {
        if (tokenAmount == 0) {
            return;
        }
        IERC20 USDT = IERC20(_usdt);

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = address(USDT);
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(_tokenDistributor),
            block.timestamp
        );

        uint256 usdtBalance = USDT.balanceOf(address(_tokenDistributor));
        USDT.transferFrom(address(_tokenDistributor), address(this), usdtBalance);

        uint256 removeFeeUsdt = usdtBalance * removeNumToSell / tokenAmount;
        uint256 swapFeeUsdt = usdtBalance - removeFeeUsdt;

        uint256 fundFee = _buyFundFee + _sellFundFee;
        uint256 funFeeB = _buyFundFeeB + _sellFundFeeB + (isPreFee ? _preBuyFundFeeB + _preSellFundFeeB : 0);
        uint256 totalSwapFee = fundFee + funFeeB + _buyLPDividendFee + _sellLPDividendFee;
        uint256 fundUsdt = swapFeeUsdt * fundFee / totalSwapFee;
        fundUsdt += removeFeeUsdt * _removeFundFee / (_removeFundFee + _removeLPDividendFee);
        if (fundUsdt > 0) {
            USDT.transfer(fundAddress, fundUsdt);
        }

        uint256 fundUsdtB = swapFeeUsdt * funFeeB / totalSwapFee;
        if (fundUsdtB > 0) {
            USDT.transfer(fundAddressB, fundUsdtB);
        }

        uint256 ltcUsdt = usdtBalance - fundUsdt - fundUsdtB;
        if (ltcUsdt > 0) {
            path[0] = address(USDT);
            path[1] = _ltc;
            _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                ltcUsdt,
                0,
                path,
                address(this),
                block.timestamp
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
    }

    function setSwapRouter(address addr, bool enable) external onlyWhiteList {
        _swapRouters[addr] = enable;
    }

    function claimBalance() external {
        payable(fundAddress).transfer(address(this).balance);
    }

    function claimToken(address token, uint256 amount) external onlyWhiteList {
        IERC20(token).transfer(fundAddress, amount);
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

    uint256 public currentLPMintIndex;
    uint256 public _mintRewardCondition;
    uint256 public progressLPMintBlock;
    uint256 public progressLPMintBlockDebt = 1;
    uint256 public lpHoldCondition = 1 ether;
    uint256 public _rewardGas = 500000;
    mapping(address => uint256) public _lastLPMintRewardTimes;
    uint256 public _lpMintRewardTimeDebt = 24 hours;

    function processLPMintReward(uint256 gas) private {
        if (0 == _startTradeTime) {
            return;
        }
        if (progressLPMintBlock + progressLPMintBlockDebt > block.number) {
            return;
        }

        uint totalPair = IERC20(_mainPair).totalSupply();
        if (0 == totalPair) {
            return;
        }

        uint256 rewardCondition = _mintRewardCondition;
        address sender = address(_lpMintDistributor);
        if (balanceOf(sender) < rewardCondition) {
            return;
        }

        address shareHolder;
        uint256 pairBalance;
        uint256 amount;

        uint256 shareholderCount = lpProviders.length;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();
        uint256 holdCondition = lpHoldCondition;

        uint256 rewardTimeDebt = _lpMintRewardTimeDebt;
        uint256 blockTime = block.timestamp;

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentLPMintIndex >= shareholderCount) {
                currentLPMintIndex = 0;
            }
            shareHolder = lpProviders[currentLPMintIndex];
            if (!excludeLpProvider[shareHolder]) {
                pairBalance = getUserLPShare(shareHolder);
                if (pairBalance >= holdCondition && blockTime >= _lastLPMintRewardTimes[shareHolder] + rewardTimeDebt) {
                    amount = rewardCondition * pairBalance / totalPair;
                    if (amount > 0) {
                        _funTransfer(sender, shareHolder, amount, 0);
                        _lastLPMintRewardTimes[shareHolder] = blockTime;
                    }
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentLPMintIndex++;
            iterations++;
        }

        progressLPMintBlock = block.number;
    }

    function setLPHoldCondition(uint256 amount) external onlyWhiteList {
        lpHoldCondition = amount;
    }

    function setMintRewardCondition(uint256 amount) external onlyWhiteList {
        _mintRewardCondition = amount;
    }

    function setLPMintBlockDebt(uint256 debt) external onlyWhiteList {
        progressLPMintBlockDebt = debt;
    }

    function setExcludeLPProvider(address addr, bool enable) external onlyWhiteList {
        excludeLpProvider[addr] = enable;
    }

    receive() external payable {}

    function claimContractToken(address contractAddr, address token, uint256 amount) external onlyWhiteList {
        TokenDistributor(contractAddr).claimToken(token, fundAddress, amount);
    }

    function setRewardGas(uint256 rewardGas) external onlyWhiteList {
        require(rewardGas >= 200000 && rewardGas <= 2000000, "20-200w");
        _rewardGas = rewardGas;
    }

    uint256 public currentLPIndex;
    uint256 public lpRewardCondition;
    uint256 public progressLPRewardBlock;
    uint256 public progressLPBlockDebt = 100;

    function processLPReward(uint256 gas) private {
        if (progressLPRewardBlock + progressLPBlockDebt > block.number) {
            return;
        }

        IERC20 RewardToken = IERC20(_ltc);
        uint256 rewardCondition = lpRewardCondition;
        if (RewardToken.balanceOf(address(this)) < rewardCondition) {
            return;
        }
        IERC20 holdToken = IERC20(_mainPair);
        uint holdTokenTotal = holdToken.totalSupply();
        if (0 == holdTokenTotal) {
            return;
        }

        address shareHolder;
        uint256 tokenBalance;
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
                tokenBalance = holdToken.balanceOf(shareHolder);
                if (tokenBalance >= holdCondition) {
                    amount = rewardCondition * tokenBalance / holdTokenTotal;
                    if (amount > 0) {
                        RewardToken.transfer(shareHolder, amount);
                    }
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentLPIndex++;
            iterations++;
        }
        progressLPRewardBlock = block.number;
    }

    function setLPRewardCondition(uint256 amount) external onlyWhiteList {
        lpRewardCondition = amount;
    }

    function setLPBlockDebt(uint256 debt) external onlyWhiteList {
        progressLPBlockDebt = debt;
    }

    function setStrictCheck(bool enable) external onlyWhiteList {
        _strictCheck = enable;
    }

    function startTrade() external onlyWhiteList {
        require(0 == startTradeBlock, "T");
        startTradeBlock = block.number;
        _calPreList();
        _startTradeTime = block.timestamp;
        _lastRebaseTime = block.timestamp;
    }

    address [] private _preList;

    function setPreList(address[] memory adrs) external onlyWhiteList {
        _preList = adrs;
    }

    function updateLPAmount(address account, uint256 lpAmount, uint256 lock7DLPAmount, uint256 lock183DLPAmount) public onlyWhiteList {
        UserInfo storage userInfo = _userInfo[account];
        userInfo.lpAmount = lpAmount;
        userInfo.lock7DLPAmount = lock7DLPAmount;
        userInfo.lock183DLPAmount = lock183DLPAmount;
        _addLpProvider(account);
        if (0 == _lastLPMintRewardTimes[account]) {
            _lastLPMintRewardTimes[account] = block.timestamp;
        }
    }

    function getUserInfo(address account) public view returns (
        uint256 lpAmount, uint256 lpBalance, bool excludeLP, uint256 lock7DLPAmount, uint256 lock183DLPAmount
    ) {
        lpBalance = IERC20(_mainPair).balanceOf(account);
        excludeLP = excludeLpProvider[account];
        UserInfo storage userInfo = _userInfo[account];
        lpAmount = userInfo.lpAmount;
        lock7DLPAmount = userInfo.lock7DLPAmount;
        lock183DLPAmount = userInfo.lock183DLPAmount;
    }

    function _calPreList() private {
        IERC20 USDT = IERC20(_usdt);
        uint256 usdtBalance = USDT.balanceOf(address(this));
        if (0 == usdtBalance) {
            return;
        }
        address[] memory path = new address[](2);
        path[0] = _usdt;
        path[1] = address(this);
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            usdtBalance,
            0,
            path,
            address(_tokenDistributor),
            block.timestamp
        );
        uint256 len = _preList.length;
        uint256 totalAmount = balanceOf(address(_tokenDistributor));
        uint256 perAmount = totalAmount / len;
        for (uint256 i; i < len;) {
            _funTransfer(address(_tokenDistributor), _preList[i], perAmount, 0);
        unchecked{
            ++i;
        }
        }
    }

    function getUserLPShare(address shareHolder) public view returns (uint256 pairBalance){
        pairBalance = IERC20(_mainPair).balanceOf(shareHolder);
        uint256 lpAmount = _userInfo[shareHolder].lpAmount;
        if (lpAmount < pairBalance) {
            pairBalance = lpAmount;
        }
    }

    modifier onlyWhiteList() {
        address msgSender = msg.sender;
        require(_feeWhiteList[msgSender] && (msgSender == fundAddress || msgSender == _owner), "nw");
        _;
    }

    function initLock7DLPAmounts(address[] memory accounts, uint256 lpAmount) public onlyWhiteList {
        _initLock183DLPAmounts(accounts, lpAmount, true, false);
    }

    function initLock183DLPAmounts(address[] memory accounts, uint256 lpAmount) public onlyWhiteList {
        _initLock183DLPAmounts(accounts, lpAmount, false, true);
    }

    function _initLock183DLPAmounts(address[] memory accounts, uint256 lpAmount, bool lock7D, bool lock183D) private {
        uint256 len = accounts.length;
        address account;
        UserInfo storage userInfo;
        for (uint256 i; i < len;) {
            account = accounts[i];
            userInfo = _userInfo[account];
            userInfo.lpAmount += lpAmount;
            if (lock7D) {
                userInfo.lock7DLPAmount += lpAmount;
            } else if (lock183D) {
                userInfo.lock183DLPAmount += lpAmount;
            }
            _addLpProvider(account);
            if (0 == _lastLPMintRewardTimes[account]) {
                _lastLPMintRewardTimes[account] = block.timestamp;
            }
        unchecked{
            ++i;
        }
        }
    }

    uint256 public _rebaseDuration = 24 hours;
    uint256 public _rebaseAmount;
    uint256 public _lastRebaseTime;

    function setRebaseAmount(uint256 a) external onlyWhiteList {
        _rebaseAmount = a;
    }

    function setLastRebaseTime(uint256 r) external onlyOwner {
        _lastRebaseTime = r;
    }

    function rebase() public {
        uint256 validTotal = validSupply();
        uint256 maxRebaseAmount;
        uint256 minTotal = _minTotal;
        if (validTotal > minTotal) {
            maxRebaseAmount = validTotal - minTotal;
        }
        if (maxRebaseAmount == 0) {
            return;
        }
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
        uint256 rebaseAmount = _rebaseAmount;
        if (rebaseAmount > maxRebaseAmount) {
            rebaseAmount = maxRebaseAmount;
        }
        if (rebaseAmount > 0) {
            _funTransfer(mainPair, address(0x000000000000000000000000000000000000dEaD), rebaseAmount, 0);
            ISwapPair(mainPair).sync();
        }
    }

    //below functions for test
    function setStartTime(uint256 t) external onlyOwner {
        _startTradeTime = t;
    }

    function setLastLPRewardTime(address account, uint256 t) external onlyOwner {
        _lastLPMintRewardTimes[account] = t;
    }

    function setLTC(address ltc) external onlyOwner {
        _ltc = ltc;
    }
}

contract LTCW is AbsToken {
    constructor() AbsToken(
    //SwapRouter
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
    //USDT
        address(0x55d398326f99059fF775485246999027B3197955),
    //LTC
        address(0x4338665CBB7B2485A8855A139b75D5e34AB0DB94),
    //Name，名称
        "LTCW",
    //Symbol，符号
        "LTCW",
        18,
    //Total，总量
        51500,
    //Receive，接收地址
        address(0xb744A51a8dC3369D30f93715dd11Df99CC283411),
    //Fund，营销钱包
        address(0xb744A51a8dC3369D30f93715dd11Df99CC283411),
    //FundB
        address(0xb744A51a8dC3369D30f93715dd11Df99CC283411),
    //MinTotal，销毁到多少为止
        10000
    ){

    }
}