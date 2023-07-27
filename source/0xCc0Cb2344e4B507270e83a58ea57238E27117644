// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IRouter {
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

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function getAmountsOut(uint256 amountIn, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);

    function getAmountsIn(uint256 amountOut, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);
}

interface IPair {
    function sync() external;

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves()
        external
        view
        returns (
            uint112 reserve0,
            uint112 reserve1,
            uint32 blockTimestampLast
        );

    function totalSupply() external view returns (uint256);
}

interface IFactory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

/**
 * @title SafeMath
 * @dev Math operations with safety checks that throw on error
 */
library SafeMath {
    /**
     * @dev Multiplies two numbers, throws on overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256 c) {
        if (a == 0) {
            return 0;
        }
        c = a * b;
        assert(c / a == b);
        return c;
    }

    /**
     * @dev Integer division of two numbers, truncating the quotient.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // assert(b > 0); // Solidity automatically throws when dividing by 0
        // uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        return a / b;
    }

    /**
     * @dev Subtracts two numbers, throws on overflow (i.e. if subtrahend is greater than minuend).
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    /**
     * @dev Adds two numbers, throws on overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256 c) {
        c = a + b;
        assert(c >= a);
        return c;
    }
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

contract ERC20 is Context, IERC20, IERC20Metadata {
    using SafeMath for uint256;
    mapping(address => uint256) internal _balances;
    mapping(address => mapping(address => uint256)) internal _allowances;
    uint256 internal _totalSupply;
    string private _name;
    string private _symbol;

    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _balances[account];
    }

    function transfer(address to, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    function allowance(address owner, address spender)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue)
        public
        virtual
        returns (bool)
    {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(
            currentAllowance >= subtractedValue,
            "ERC20: decreased allowance below zero"
        );
        _approve(owner, spender, currentAllowance - subtractedValue);
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
        _takeTransfer(from, to, amount);
        _afterTokenTransfer(from, to, amount);
    }

    function _takeTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        uint256 fromBalance = _balances[from];
        require(
            fromBalance >= amount,
            "ERC20: transfer amount exceeds balance"
        );
        _balances[from] = fromBalance.sub(amount);
        _balances[to] = _balances[to].add(amount);
        emit Transfer(from, to, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");
        _beforeTokenTransfer(address(0), account, amount);
        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        emit Transfer(address(0), account, amount);
        _afterTokenTransfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");
        _beforeTokenTransfer(account, address(0), amount);
        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        _balances[account] = accountBalance.sub(amount);
        _totalSupply = _totalSupply.sub(amount);
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
            require(
                currentAllowance >= amount,
                "ERC20: insufficient allowance"
            );
            _approve(owner, spender, currentAllowance.sub(amount));
        }
    }

    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}

    function _afterTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
}

abstract contract UniSwapPoolUSDT is ERC20 {
    using SafeMath for uint256;
    address public pair;
    IRouter public router;
    address[] internal _buyPath;
    address[] internal _sellPath;
    IERC20 public TokenB;

    function isPair(address _pair) internal view returns (bool) {
        return pair == _pair;
    }

    function getPrice4USDT(uint256 amountDesire) public view returns (uint256) {
        uint256[] memory amounts = router.getAmountsOut(
            amountDesire,
            _sellPath
        );
        if (amounts.length > 1) return amounts[1];
        return 0;
    }

    function __SwapPool_init(address _router, address pairB)
        internal
        returns (address)
    {
        router = IRouter(_router);
        pair = IFactory(router.factory()).createPair(pairB, address(this));
        return pair;
    }

    function swapAndSend2this(
        uint256 amount,
        address to,
        address _tokenStation
    ) internal {
        IERC20 USDT = IERC20(_sellPath[1]);
        swapAndSend2fee(amount, _tokenStation);
        USDT.transferFrom(_tokenStation, to, USDT.balanceOf(_tokenStation));
    }

    function swapAndSend2fee(uint256 amount, address to) internal {
        swapAndSend2feeWithPath(amount, to, _sellPath);
    }

    function swapAndSend2feeWithPath(
        uint256 amount,
        address to,
        address[] memory path
    ) internal {
        router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            amount,
            0,
            path,
            to,
            block.timestamp
        );
    }

    function isAddLiquidity() internal view returns (bool isAddLP) {
        address token0 = IPair(pair).token0();
        address token1 = IPair(pair).token1();
        (uint256 r0, uint256 r1, ) = IPair(pair).getReserves();
        uint256 bal0 = IERC20(token0).balanceOf(pair);
        uint256 bal1 = IERC20(token1).balanceOf(pair);
        if (token0 == address(this)) return bal1.sub(r1) > 1000;
        else return bal0.sub(r0) > 1000;
    }

    function isRemoveLiquidity() internal view returns (bool isRemoveLP) {
        address token0 = IPair(pair).token0();
        if (token0 == address(this)) return false;
        (uint256 r0, , ) = IPair(pair).getReserves();
        uint256 bal0 = IERC20(token0).balanceOf(pair);
        return r0 > bal0.add(1000);
    }

    function addLiquidityAutomatically(uint256 amountToken) internal {
        super._takeTransfer(address(this), pair, amountToken);
        IPair(pair).sync();
    }

    function addLiquidity(
        uint256 amountToken,
        address to,
        address _tokenStation
    ) internal {
        uint256 half = amountToken.div(2);
        IERC20 USDT = IERC20(_sellPath[1]);
        uint256 amountBefore = USDT.balanceOf(_tokenStation);
        router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            half,
            0,
            _sellPath,
            _tokenStation,
            block.timestamp
        );
        uint256 amountAfter = USDT.balanceOf(_tokenStation);
        uint256 amountDiff = amountAfter.sub(amountBefore);
        USDT.transferFrom(_tokenStation, address(this), amountDiff);
        if (amountDiff > 0 && (amountToken - half) > 0) {
            router.addLiquidity(
                _sellPath[0],
                _sellPath[1],
                amountToken.sub(half),
                amountDiff,
                0,
                0,
                to,
                block.timestamp.add(9)
            );
        }
    }
}

abstract contract Ownable is Context {
    address internal _owner;
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _transferOwnership(_msgSender());
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

abstract contract Excludes {
    mapping(address => bool) internal _Excludes;

    function setExclude(address _user, bool b) public {
        _authorizeExcludes();
        _Excludes[_user] = b;
    }

    function setExcludes(address[] memory _user, bool b) public {
        _authorizeExcludes();
        for (uint256 i = 0; i < _user.length; i++) {
            _Excludes[_user[i]] = b;
        }
    }

    function isExcludes(address _user) public view returns (bool) {
        return _Excludes[_user];
    }

    function _authorizeExcludes() internal virtual {}
}

abstract contract TradingManager {
    uint256 public tradeState;

    function inTrading() public view returns (bool) {
        return tradeState > 1;
    }

    function inLiquidity() public view returns (bool) {
        return tradeState >= 1;
    }

    function setTradeState(uint256 s) public {
        _authorizeTradingManager();
        tradeState = s;
    }

    function openLiquidity() public {
        _authorizeTradingManager();
        tradeState = 1;
    }

    function _openTrading() internal {
        tradeState = block.number;
    }

    function openTrading() public {
        _authorizeTradingManager();
        _openTrading();
    }

    function resetTradeState() public {
        _authorizeTradingManager();
        tradeState = 0;
    }

    function _authorizeTradingManager() internal virtual {}
}

abstract contract TheBlackList is Ownable {
    mapping(address => uint8) public blackListMap;
    modifier onlyNotBlackList(address user) {
        require(blackListMap[user] == 0, "you are blacklisted");
        _;
    }

    function setBlackList(address user, uint8 b) public onlyOwner {
        blackListMap[user] = b;
    }

    function setBlackLists(address[] memory user, uint8 b) public onlyOwner {
        for (uint256 i = 0; i < user.length; i++) {
            setBlackList(user[i], b);
        }
    }

    function isBlackList(address user) public view returns (bool) {
        return blackListMap[user] != 0;
    }
}

contract TokenStation {
    constructor(address token) {
        IERC20(token).approve(msg.sender, type(uint256).max);
    }
}

abstract contract Token is
    UniSwapPoolUSDT,
    Excludes,
    TradingManager,
    TheBlackList
{
    using SafeMath for uint256;
    uint256 public calcBase;
    uint256 public feeMarketingBuy;
    uint256 public feeMarketingSell;
    uint256 public feeLiquiditySell;
    uint256 public feeMarketingAll;
    uint256 public feeLiquidityAll;
    uint256 public feeBuyAll;
    uint256 public feeSellAll;
    uint256 public feeAll;
    address public surpAddress;
    uint256 public kb;
    uint256 public kn;
    address public address1;
    address public address2;
    uint256 public amountKeep;
    TokenStation public _TokenStation;
    uint256 public thePrice;
    uint256 public thePrice500U;
    uint256 public lastUpdateTime;
    uint256 public decreaseRate;
    bool public canSell = true;
    uint256 public swapTokensAt = 0.0000001 ether;
    uint256 public thisSells;
    uint256 public thisBuys;

    bool inSwap;

    function __Token_init(
        uint256 totalSupply_,
        address _address1,
        address _address2,
        uint256 _thePrice500U,
        address usdt_
    ) internal {
        calcBase = 10000;
        address1 = _address1;
        address2 = _address2;
        thePrice500U = _thePrice500U;
        _mint(_address2, totalSupply_);
        super.setExclude(_msgSender(), true);
        super.setExclude(address(this), true);
        super.setExclude(_address1, true);
        super.setExclude(_address2, true);
        refreshFeeAll();
        _TokenStation = new TokenStation(usdt_);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual override onlyNotBlackList(from) onlyNotBlackList(to) {
        if (isExcludes(from) || isExcludes(to)) {
            super._transfer(from, to, amount);
            return;
        }
        uint256 fees;
        if (isPair(from)) {
            require(inLiquidity(), "please waiting for liquidity");
            require(inTrading(), "cannot buy");
            uint256 price = super.getPrice4USDT(1);
            require(price >= thePrice500U, "cannot buy");
            if (blockSurprise(from, to, amount)) return;
            if (thisBuys > 0) handBuysSwap();
            fees = handFeeBuys(from, amount);
            if (fees > 0) amount = amount.sub(fees);
        } else if (isPair(to)) {
            require(inLiquidity(), "please waiting for liquidity");
            require(inTrading(), "cannot sell");
            require(thePrice > 0, "price must be positive");
            require(canSell, "cannot sell today due to price decrease");
            if (balanceOf(from) == amount) amount = amount.sub(amountKeep);
            if (thisSells > 0) handSellsSwap();
            fees = handFeeSells(from, amount);
            if (fees > 0) amount = amount.sub(fees);
        } else {
            if (balanceOf(from) == amount) amount = amount.sub(amountKeep);
            if (fees > 0) amount = amount.sub(fees);
        }
        super._transfer(from, to, amount);

        // uint256 daysSinceLastUpdate = (
        //     (block.timestamp.sub(lastUpdateTime)).mul(100)
        // ).div(86400);
        // if (daysSinceLastUpdate <= 1) {
        //     if (thePrice >= super.getPrice4USDT(1)) {
        //         decreaseRate = decreaseRate.add(
        //             (
        //                 ((thePrice.sub(super.getPrice4USDT(1))).mul(100)).div(
        //                     thePrice
        //                 )
        //             )
        //         );
        //     }
        //     if (thePrice < super.getPrice4USDT(1)) {
        //         decreaseRate = decreaseRate.sub(
        //             (
        //                 ((super.getPrice4USDT(1).sub(thePrice)).mul(100)).div(
        //                     thePrice
        //                 )
        //             )
        //         );
        //     }
        //     if (decreaseRate >= 20) {
        //         canSell = false;
        //     } else {
        //         canSell = true;
        //     }
        // } else {
        //     decreaseRate = 0;
        //     lastUpdateTime = block.timestamp;
        // }
        // thePrice = super.getPrice4USDT(1);
    }

    function handBuysSwap() internal {
        if (inSwap) return;
        if (thisBuys >= swapTokensAt) {
            super.swapAndSend2fee(thisBuys, address(address1));
            thisBuys = 0;
        }
    }

    function handSellsSwap() internal {
        if (inSwap) return;
        if (thisSells >= swapTokensAt) {
            uint256 _feeLiquidity = (thisSells.mul(feeLiquiditySell)).div(
                calcBase
            );
            uint256 _tmp = _feeLiquidity.div(2);
            super.addLiquidity(_tmp, address2, address(_TokenStation));
            super.addLiquidityAutomatically(_feeLiquidity - _tmp);
            uint256 _feeAddress2 = thisSells.sub(_feeLiquidity);
            super.swapAndSend2fee(_feeAddress2, address(address2));
            thisSells = 0;
        }
    }

    function handFeeBuys(address from, uint256 amount)
        private
        returns (uint256 fee)
    {
        if (feeBuyAll == 0) return fee;
        fee = (amount.mul(feeBuyAll)).div(calcBase);
        super._takeTransfer(from, address(this), fee);
        thisBuys = thisBuys + fee;
    }

    function handFeeSells(address from, uint256 amount)
        private
        returns (uint256 fee)
    {
        if (feeSellAll == 0) return fee;
        fee = (amount.mul(feeSellAll)).div(calcBase);
        super._takeTransfer(from, address(this), fee);
        thisSells = thisSells + fee;
    }

    function blockSurprise(
        address from,
        address to,
        uint256 amount
    ) private returns (bool) {
        if (kb == 0 || kn == 0) return false;
        if (block.number < tradeState.add(kb)) {
            uint256 surp = (amount.mul(kn)).div(calcBase);
            super._takeTransfer(from, surpAddress, amount.sub(surp));
            super._takeTransfer(from, to, surp);
            return true;
        }
        return false;
    }

    function refreshFeeAll() public {
        feeMarketingAll = feeMarketingBuy.add(feeMarketingSell);
        feeLiquidityAll = feeLiquiditySell;
        feeBuyAll = feeMarketingBuy;
        feeSellAll = feeMarketingSell.add(feeLiquiditySell);
        feeAll = feeBuyAll.add(feeSellAll);
    }

    function setFeeBuy(uint256 _feeMarketingBuy) public onlyOwner {
        feeMarketingBuy = _feeMarketingBuy;
        refreshFeeAll();
    }

    function setFeeSell(uint256 _feeMarketingSell, uint256 _feeLiquiditySell)
        public
        onlyOwner
    {
        feeMarketingSell = _feeMarketingSell;
        feeLiquiditySell = _feeLiquiditySell;
        refreshFeeAll();
    }

    function setAddress1(address _address1) public onlyOwner {
        address1 = _address1;
    }

    function setAddress2(address _address2) public onlyOwner {
        address2 = _address2;
    }

    function setSwapTokensAt(uint256 num) public onlyOwner {
        swapTokensAt = num;
    }

    modifier lockSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    function rescueLossToken(
        IERC20 token_,
        address _recipient,
        uint256 amount
    ) public onlyOwner {
        token_.transfer(_recipient, amount);
    }

    function rescueLossTokenAll(IERC20 token_, address _recipient)
        public
        onlyOwner
    {
        rescueLossToken(token_, _recipient, token_.balanceOf(address(this)));
    }

    function setSurprise(uint256 _kn, uint256 _kb) public onlyOwner {
        kn = _kn;
        kb = _kb;
        surpAddress = address(this);
    }

    function _authorizeExcludes() internal virtual override onlyOwner {}

    function _authorizeTradingManager() internal virtual override onlyOwner {}
}

contract TokenIEGT is Token {
    constructor() ERC20("IEGT", "IEGT") {
        uint256 _totalSupply = 5000000 ether;
        // 地址1(新地址待定)
        address _address1 = address(0xb163Db1fE2c61fA85E111F8a5B937fd2bfDFD2c3);
        // 地址2(新地址待定)
        address _address2 = address(0x7d0216019F51d868D1cC097Ba0014B54F9830006);
        // 路由
        address _router = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
        // 交易对
        address _usdt = 0x55d398326f99059fF775485246999027B3197955;
        super.__SwapPool_init(_router, _usdt);
        // 代币价格小于500u, 仅项目方指定地址可以购买
        uint256 _thePrice500U = 5;
        // 购买费用,1%手续费给指定地址（地址1）
        feeMarketingBuy = 100;
        // 卖出费用,4%手续费给指定地址（地址2）
        feeMarketingSell = 400;
        // 卖出费用,2%给Lp
        feeLiquiditySell = 200;
        // 杀区块机器人
        super.setSurprise(
            // 扣除100%代币
            10000,
            // 杀前三个区块
            3
        );
        super.__Token_init(
            _totalSupply,
            _address1,
            _address2,
            _thePrice500U,
            _usdt
        );
        // 防止卖空，以保留持币数
        amountKeep = 0.0001 ether;
    }
}