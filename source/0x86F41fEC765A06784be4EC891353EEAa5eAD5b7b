// SPDX-License-Identifier: MIT

pragma solidity 0.8.17;

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
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

interface ISwapFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
    function getPair(address tokenA, address tokenB) external view returns (address pair);
}

interface IUniswapV2Pair {
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function sync() external;
}

interface ILpHolder {
    function distributeUsdt(uint256 usdtAmount, uint256 lpAmount) external;
}

interface IDapp {
    function clearOrder(address userAddress) external;
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

abstract contract BaseToken is IERC20, Ownable {
    
    event ProcessLP(uint64 time);

    bool private inSwap;
    uint8 private _decimals;  
    uint32 public _startTradeBlock;
    uint32 public _currentIndex;
    uint32 public _progressLPTime;

    uint256 private _totalSupply;
    uint256 private constant MAX = ~uint256(0);
    uint256 public _waitForSwapAmount;
    uint256 public _waitForSwapCutAmount;
    uint256 public _waitForBackAmount;
    uint256 public _limitAmount;
    uint256 public _addPriceTokenAmount; 
    uint256 public _lastPrice;
    uint256 public _priceTime;

    ISwapRouter private _swapRouter;
    TokenDistributor public _tokenDistributor;
    IDapp public _dapp;
    address private _marketAddress;
    address private _usdtAddress;
    address private _mainPairAddress;
    address public _lpHolderContractAddress;

    string private _name;
    string private _symbol;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _feeWhiteList;
    mapping(address => bool) private _swapPairMap;
    mapping(address => uint256) _lpProviderIndex;
    mapping(address => bool) _excludeLpProvider;    
    mapping(address=>uint256) public _userHoldPrice;


    address[] private _lpProviders;

    constructor (string memory Name, string memory Symbol, uint256 Supply, address RouterAddress, address UsdtAddress, address marketAddress, address dappAddress){
        _name = Name;
        _symbol = Symbol;
        _decimals = 18;
        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        _usdtAddress = UsdtAddress;
        _swapRouter = swapRouter;
        _allowances[address(this)][RouterAddress] = MAX;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        _mainPairAddress = swapFactory.createPair(address(this), UsdtAddress);
        _swapPairMap[_mainPairAddress] = true;

        uint256 total = Supply * 1e18;
        _totalSupply = total;


        _balances[0xc72724220E357a818E40a3996148f66A22309740] = 63e23; 
        emit Transfer(address(0), 0xc72724220E357a818E40a3996148f66A22309740, 63e23);   
        _feeWhiteList[0xc72724220E357a818E40a3996148f66A22309740] = true;

        _balances[0x95Fa3f75Fb2e8C82A284a77D88F86926d368dEBb] = 105e22; 
        emit Transfer(address(0), 0x95Fa3f75Fb2e8C82A284a77D88F86926d368dEBb, 105e22);
        _feeWhiteList[0x95Fa3f75Fb2e8C82A284a77D88F86926d368dEBb] = true;

        _balances[0xa335733C1CE6a15Cc0B9E0B7F033a6B75a759be3] = 1134e22; 
        emit Transfer(address(0), 0xa335733C1CE6a15Cc0B9E0B7F033a6B75a759be3, 1134e22);
        _feeWhiteList[0xa335733C1CE6a15Cc0B9E0B7F033a6B75a759be3] = true;

        _balances[0x87EB3f3a0AC46e3DEcfC48c2F9E54A5D20C27598] = 105e22; 
        emit Transfer(address(0), 0x87EB3f3a0AC46e3DEcfC48c2F9E54A5D20C27598, 105e22);
        _feeWhiteList[0x87EB3f3a0AC46e3DEcfC48c2F9E54A5D20C27598] = true;

        _balances[0x4bE49E687bF9f41c3f017baB05A777702563aA2B] = 21e22; 
        emit Transfer(address(0), 0x4bE49E687bF9f41c3f017baB05A777702563aA2B, 21e22);     
        _addLpProvider(0x4bE49E687bF9f41c3f017baB05A777702563aA2B); 
        _feeWhiteList[0x4bE49E687bF9f41c3f017baB05A777702563aA2B] = true;

        
        _balances[msg.sender] = 21e22; 
        emit Transfer(address(0), msg.sender, 21e22);     
        _addLpProvider(msg.sender); 

        _marketAddress = marketAddress;

        _balances[dappAddress] = 105e22; 
        emit Transfer(address(0), dappAddress, 105e22);
        _feeWhiteList[dappAddress] = true;


        _dapp = IDapp(dappAddress);

        _marketAddress = marketAddress;

        _feeWhiteList[marketAddress] = true;
        _feeWhiteList[dappAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;
        _feeWhiteList[address(0x000000000000000000000000000000000000dEaD)] = true;

        _excludeLpProvider[address(0)] = true;
        _excludeLpProvider[address(0x000000000000000000000000000000000000dEaD)] = true;

        _limitAmount = 10*1e18;
        _addPriceTokenAmount = 1e14;
        _tokenDistributor = new TokenDistributor(UsdtAddress);
        _startTradeBlock = 0;
    }

    function pairAddress() external view returns (address) {
        return _mainPairAddress;
    }
    
    function routerAddress() external view returns (address) {
        return address(_swapRouter);
    }
    
    function usdtAddress() external view returns (address) {
        return _usdtAddress;
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

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) external view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
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

    
    function _isLiquidity(address from,address to) internal view returns(bool isAdd,bool isDel){
        address token0 = IUniswapV2Pair(_mainPairAddress).token0();
        (uint r0,,) = IUniswapV2Pair(_mainPairAddress).getReserves();
        uint bal0 = IERC20(token0).balanceOf(_mainPairAddress);
        if( _swapPairMap[to] ){
            if( token0 != address(this) && bal0 > r0 ){
                isAdd = bal0 - r0 > _addPriceTokenAmount;
            }
        }
        if( _swapPairMap[from] ){
            if( token0 != address(this) && bal0 < r0 ){
                isDel = r0 - bal0 > 0; 
            }
        }
    }

    function updateLastPrice() public {
        uint256 newTime = block.timestamp/86400; 
        if(newTime > _priceTime){
            _lastPrice = getNowPrice();
            _priceTime = newTime;
        }
    }

    function getNowPrice() internal view returns(uint256){
        uint256 poolToken = _balances[_mainPairAddress];
        if(poolToken > 0){
            return IERC20(_usdtAddress).balanceOf(_mainPairAddress)*1e18/poolToken;
        }
        return 0;
    }

    function getDownRate() public view returns(uint256){ 
        if(_lastPrice > 0){
            uint256 nowPrice = getNowPrice();
            if(_lastPrice > nowPrice){
                return (_lastPrice - nowPrice)*100/_lastPrice;
            }
        }
        return 0;
    }

    function getCutRate() public view returns(uint256){
        uint256 downRate = getDownRate();
        if(downRate >= 50){
            return 40;
        }else if(downRate >= 30){
            return 20;
        }else{
            return 0;
        }
    }

    function getHolderCutAmount(address user,uint256 amount,uint256 currentPrice) public view returns(uint256){
        uint256 holderPrice = _userHoldPrice[user];
        if(holderPrice > 0 && currentPrice > holderPrice){
            uint256 ylcount= amount*(currentPrice - holderPrice)/holderPrice;
            return ylcount/10; 
        }
        return 0;
    }

    function updateHolderPrice(address user, uint256 currentprice, uint256 amount) public {        
        uint256 oldbalance= _balances[user]; 
        uint256 totalvalue = _userHoldPrice[user]*oldbalance; 
        totalvalue += amount*currentprice;
        _userHoldPrice[user] = totalvalue/(oldbalance+amount);
    }

    function getCurrentPrice() external view returns (uint256){
        return getNowPrice();
    }

    bool public _swapAtBuy=false;
    bool public _swapAtSell=false;

    function setSwapAtBuy(bool enable) external{
        _swapAtBuy = enable;
    }

    function setSwapAtSell(bool enable) external{
        _swapAtSell = enable;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {       
        require(amount > 0, "KL: transfer amount must be >0");
        if(address(this)==from) {
            _tokenTransfer(from, to, amount); 
            return;
        }
        bool isAddLiquidity;
        bool isDelLiquidity;
        ( isAddLiquidity, isDelLiquidity) = _isLiquidity(from,to);
        
        if (_feeWhiteList[from] || _feeWhiteList[to] || isAddLiquidity || isDelLiquidity){
            
            _tokenTransfer(from, to, amount);

            if(!isAddLiquidity && !isDelLiquidity){
                
                swapUSDT();
                if(_waitForBackAmount > _limitAmount){
                    _tokenTransfer(address(this), _mainPairAddress, _waitForBackAmount); 
                    _waitForBackAmount = 0;
                }
            }
        }else if(_swapPairMap[from] || _swapPairMap[to]){
            
            require(_startTradeBlock > 0, "KL: trade don't start");      
            uint256 currentprice= getNowPrice();
            updateLastPrice();    
            if (_swapPairMap[to]) { 
                
                require(amount <= (_balances[from])*99/100, "KL: sell amount exceeds balance 99%");  
                
                
                swapUSDT();

                
                uint256 cutAmount = getHolderCutAmount(from,amount,currentprice);
                if(cutAmount > 0){
                    _tokenTransfer(from, address(this), cutAmount) ; 
                    _waitForSwapCutAmount += cutAmount;
                    amount -= cutAmount;
                }

                uint256 cutRate = getCutRate();    
                if(cutRate > 0) {
                    cutRate -= 6;
                    _tokenTransfer(from, address(this), amount*cutRate/100) ; 
                    _waitForSwapCutAmount += amount*cutRate/100;

                    _tokenTransfer(from, to, amount*(100-cutRate)/100); 
                }else{
                    _tokenTransfer(from, to, amount*94/100); 
                }
                
            }else{  
                updateHolderPrice(to, currentprice, amount*94/100);  
                 _tokenTransfer(from, to, amount*94/100); 
                 
            }   
            
            
            _tokenTransfer(from, address(_dapp), amount/50); 
            _tokenTransfer(from, address(this), amount*35/1000); 
            _tokenTransfer(from, address(0), amount/200); 
            _waitForSwapAmount += amount*3/100; 
            _waitForBackAmount += amount/200; 
            
             
        }else{
            
            swapUSDT() ;
            if(_waitForBackAmount > _limitAmount){
                _tokenTransfer(address(this), _mainPairAddress, _waitForBackAmount); 
                _waitForBackAmount = 0;
            }
            updateHolderPrice(to, getNowPrice(), amount/2); 
            _tokenTransfer(from, to, amount/2);
            _tokenTransfer(from, address(0), amount/2); 
        }
        if (isAddLiquidity) { 
            _addLpProvider(from); 
        }
        if(isDelLiquidity){
            _dapp.clearOrder(to); 
        }
        _processLP(500000);
    }
    
    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        _balances[recipient] = _balances[recipient] + tAmount;
        emit Transfer(sender, recipient, tAmount);
    }

    function swapUSDT() public  {
        uint256 total = _waitForSwapAmount + _waitForSwapCutAmount;
        if(total < _limitAmount) return;
        address tokenDistributor = address(_tokenDistributor);
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _usdtAddress;
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            total,
            0,
            path,
            tokenDistributor,
            block.timestamp
        );        
        IERC20 USDT = IERC20(_usdtAddress);
        uint256 usdtBalance = USDT.balanceOf(tokenDistributor);
        uint256 cutAmount = usdtBalance * _waitForSwapCutAmount/total;
        uint256 rewardAmount = usdtBalance - cutAmount;
        _waitForSwapAmount = 0;
        _waitForSwapCutAmount = 0;
        USDT.transferFrom(tokenDistributor, _marketAddress, cutAmount + rewardAmount/3); 
        USDT.transferFrom(tokenDistributor, address(this), rewardAmount*2/3); 
    }

    function _addLpProvider(address addr) private {
        if (0 == _lpProviderIndex[addr]) {
            if (0 == _lpProviders.length || _lpProviders[0] != addr) {
                _lpProviderIndex[addr] = _lpProviders.length;
                _lpProviders.push(addr);
            }
        }
    }

    function getLps() external view returns(address [] memory){
        return _lpProviders;
    }

    function processLP(uint256 gas) external onlyOwner{
        _processLP(gas);
    }

    function _processLP(uint256 gas) internal {
        uint256 timestamp = block.timestamp;
        if (_progressLPTime + 3600 > timestamp) {
            return;
        }
        IERC20 mainpair = IERC20(_mainPairAddress);
        uint totalPair = mainpair.totalSupply();
        if (0 == totalPair) {
            return;
        }

        IERC20 USDT = IERC20(_usdtAddress);
        uint256 usdtTokenBalance = USDT.balanceOf(address(this));
        if (usdtTokenBalance < _limitAmount) {
            return;
        }

        address lpHolder;
        uint256 pairBalance;
        uint256 amount;

        uint256 lpHolderCount = _lpProviders.length;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();

        
        address lpHolderContractAddress = _lpHolderContractAddress;

        while (gasUsed < gas && iterations < lpHolderCount) {
            if (_currentIndex >= lpHolderCount) {
                _currentIndex = 0;
            }
            lpHolder = _lpProviders[_currentIndex];
            pairBalance = mainpair.balanceOf(lpHolder);
            if (pairBalance > 0 && !_excludeLpProvider[lpHolder]) {
                amount = usdtTokenBalance * pairBalance / totalPair;
                if (amount > 0) {
                    USDT.transfer(lpHolder, amount);
                    if(lpHolder == lpHolderContractAddress){
                        ILpHolder(lpHolderContractAddress).distributeUsdt(amount, pairBalance);
                    }
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            _currentIndex++;
            iterations++;
        }

        _progressLPTime = uint32(timestamp);
        emit ProcessLP(_progressLPTime);
    }

    function setLimitAmount(uint256 amount) external onlyOwner {
        _limitAmount = amount * 1e18;
    }

    function setLpHolderContractAddress(address lpHolderContractAddress) external onlyOwner {
        _lpHolderContractAddress = lpHolderContractAddress;
        _addLpProvider(lpHolderContractAddress);
    }

    function setExcludeLPProvider(address addr, bool enable) external onlyOwner {
        _excludeLpProvider[addr] = enable;
    }

    function setProgressLPTime(uint32 time) external onlyOwner {
        _progressLPTime=time;
    }

    function manulAddLpProvider(address addr) external{
        require(msg.sender==owner() || msg.sender == _lpHolderContractAddress,"KL: caller should be owner or LpHolder");
        _addLpProvider(addr);
    }

    function setMarketAddress(address addr) external onlyOwner {
        _marketAddress = addr;
        _feeWhiteList[addr] = true;
    }

    function getMarketAddress() external view returns(address) {
        return _marketAddress;
    }

    function setDappAddress(address addr) external onlyOwner {
        _dapp = IDapp(addr);
        _feeWhiteList[addr] = true;
    }

    function startTrade() external onlyOwner {
        require(0 == _startTradeBlock, "trading");
        _startTradeBlock = uint32(block.number);
    }

    function closeTrade() external onlyOwner {
        _startTradeBlock = 0;
    }

    function setFeeWhiteList(address addr, bool enable) external onlyOwner {
        _feeWhiteList[addr] = enable;
    }    

    function setSwapPairMap(address addr, bool enable) external onlyOwner {
        _swapPairMap[addr] = enable;
    }

    function setAddPriceTokenAmount(uint addPriceTokenAmount) external onlyOwner{
        _addPriceTokenAmount = addPriceTokenAmount;
    }

    receive() external payable {}
}

contract Kunlun is BaseToken {
    constructor(address dappAddress) BaseToken(
        "Kunlun coin",
        "KL",
        21000000,
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E), 
        address(0x55d398326f99059fF775485246999027B3197955), 
        address(0x4441919cdcd32eC86dDB7BeDa09F0EC47aF1aFA3), 
        dappAddress
    ){

    }
}