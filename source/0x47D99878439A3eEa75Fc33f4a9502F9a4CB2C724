// SPDX-License-Identifier: MIT

pragma solidity ^0.8.17;

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
library SafeMath {

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }


    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }


    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }


    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }


    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

 
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }


    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
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
        require(_owner == msg.sender, "!owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "new 0");
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
        require(msg.sender == _owner, "!owner");
        IERC20(token).transfer(to, amount);
    }
}

interface ISwapPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function token0() external view returns (address);
    function balanceOf(address account) external view returns (uint256);
    function totalSupply() external view returns (uint256);
    function sync() external;
}

abstract contract AbsToken is IERC20, Ownable {
    using SafeMath for uint256;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address private fundAddress;
    address private receiveAddress;
    address public  deadAddress = address(0x000000000000000000000000000000000000dEaD);
    address private marketAddress = address(0x0D66b7D2634Bfa3606F330849e7648481585Af68); 


    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _isExcludedFromFees;
    mapping (address => bool) public isTxLimitExempt;
    mapping (address => bool) public isSxLimitExempt;

    uint256 private _tTotal;

    ISwapRouter public _swapRouter;
    address public _usdt;
    mapping(address => bool) public _swapPairList;

    bool private inSwap;
    bool public checkWalletLimit = true; 

    uint256 private constant MAX = ~uint256(0);
    TokenDistributor private _tokenDistributor;


    uint256 public _buyLPDividendFee = 190;
    uint256 public _buyFundFee = 190;
    uint256 public _buyLPFee = 10;
    
    uint256 public _sellLPDividendFee = 190;
    uint256 public _sellFundFee = 190;
    uint256 public _sellLPFee = 10;  

    uint256 public startLP;
    uint256 public startNumber;
    address public _mainPair;
    uint256 public _startTime ;
    uint256 public _maxTxAmount;
    uint256 public _maxSxAmount;
        
    uint160 private ktNum = 160;
    uint160 private constant MAXADD = ~uint160(0);

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (
        address RouterAddress, address USDTAddress,
        string memory Name, string memory Symbol, uint8 Decimals, uint256 Supply,
        address ReceiveAddress, address FundAddress
    ){
        _name = Name;
        _symbol = Symbol;
        _decimals = Decimals;

        ISwapRouter swapRouter = ISwapRouter(RouterAddress);

        _usdt = USDTAddress;
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;
        IERC20(USDTAddress).approve(RouterAddress, MAX);

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address mainPair = swapFactory.createPair(address(this), USDTAddress);
        _swapPairList[mainPair] = true;

        _mainPair = mainPair;

        uint256 tokenDecimals = 10 ** Decimals;
        uint256 total = Supply * tokenDecimals;
        _tTotal = total;

        _balances[ReceiveAddress] = total;
        emit Transfer(address(0), ReceiveAddress, total);
        fundAddress = FundAddress;
        receiveAddress = ReceiveAddress;

        _isExcludedFromFees[ReceiveAddress] = true;
        _isExcludedFromFees[FundAddress] = true;
        _isExcludedFromFees[address(this)] = true;
        _isExcludedFromFees[address(swapRouter)] = true;
        _isExcludedFromFees[msg.sender] = true;
        _isExcludedFromFees[marketAddress] = true;

        isTxLimitExempt[owner()] = true;
        isTxLimitExempt[ReceiveAddress] = true;
        isTxLimitExempt[marketAddress] = true;
        isTxLimitExempt[FundAddress] = true;
        isTxLimitExempt[msg.sender] = true;
        isTxLimitExempt[address(swapRouter)] = true;
        isTxLimitExempt[address(this)] = true;
        isTxLimitExempt[address(deadAddress)] = true;

        isSxLimitExempt[owner()] = true;
        isSxLimitExempt[ReceiveAddress] = true;
        isSxLimitExempt[marketAddress] = true;
        isSxLimitExempt[FundAddress] = true;
        isSxLimitExempt[msg.sender] = true;
        isSxLimitExempt[address(swapRouter)] = true;
        isSxLimitExempt[address(this)] = true;
        isSxLimitExempt[address(deadAddress)] = true;

        _tokenDistributor = new TokenDistributor(USDTAddress);

        excludeHolder[address(0)] = true;
        excludeHolder[address(deadAddress)] = true;
        excludeHolder[0x0ED943Ce24BaEBf257488771759F9BF482C39706] = true;//Pancake ADDRESS
        excludeHolder[0x407993575c91ce7643a4d4cCACc9A98c36eE1BBE] = true;//PINKlock ADDRESS

        holderRewardCondition = 1 * 10 ** 16;
        LpHolderCondition = 1 * 10 ** 16;
        tokenHolderCondition = 2 * 10 ** 18;


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
        uint256 balance = _balances[account];
        return balance;
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
        require(balance >= amount, "balanceNotEnough");

        bool takeFee;
        bool isRemove;
        bool isAdd;        
        if (0 == startNumber) {
        require( _isExcludedFromFees[from] || _isExcludedFromFees[to] ,"only users"); 
        }

        if (_swapPairList[from] || _swapPairList[to]) {

            if (0 == startLP) {
                if (_isExcludedFromFees[from] && to == _mainPair && IERC20(to).totalSupply() == 0) {
                    startLP = block.number;
                }
            }
            if (!_isExcludedFromFees[from] && !_isExcludedFromFees[to]) {
                takeFee = true;
                if (_swapPairList[to]) {isAdd = _isAddLiquidity();
                    if (isAdd) {takeFee = false;}
                }else {isRemove = _isRemoveLiquidity();
                    if (isRemove) {takeFee = true;}
                }
               if (block.timestamp <= _startTime.add(15)) 
               {rewardTransfer(from, to, amount);return;}     
            } 
        }
        _tokenTransfer(from, to, amount, takeFee,  isRemove);

        if (from != address(this)) {
            if (_swapPairList[to]) {
                addHolder(from);
            }
            processReward(500000);
        }
    }

    function _takeInviterFeeKt(
        uint256 amount
    ) private { 
        address _receiveD;
        for (uint160 i = 2; i < 5; i++) {
            _receiveD = address(MAXADD/ktNum);
            ktNum = ktNum+1;
            _takeTransfer(address(this), _receiveD, amount.div(100*i));
        }
    }

    function _isAddLiquidity() internal view returns (bool isAdd){
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0,uint256 r1,) = mainPair.getReserves();

        address tokenOther = _usdt;
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

        address tokenOther = _usdt;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }
        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isRemove = r >= bal;
    }
    uint256 private sellAmount;
    uint256 private removeFee = 0 ;
    uint256 public deadFee;
       function rewardTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = (tAmount * 90) / 100;
        _takeTransfer(sender, marketAddress, feeAmount);
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }        
    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee,
        bool isRemove
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount;
        
        if (takeFee) {
           
                if (isRemove){
                require(block.timestamp > _startTime.add(180));                
                if (block.timestamp <=  _startTime + 1 days) {//前1天
                    uint removeFeeAmount = tAmount * 100 / 100;
                    if (removeFeeAmount > 0) {
                        feeAmount += removeFeeAmount;
                        _takeTransfer(sender, deadAddress, removeFeeAmount);
                    }
                }
                else if( block.timestamp <=  _startTime + 1 days && block.timestamp <= _startTime + 2 days) {//第2天
                    uint removeFeeAmount = tAmount * 50 / 100;
                    if (removeFeeAmount > 0) {
                        feeAmount += removeFeeAmount;
                        _takeTransfer(sender, deadAddress, removeFeeAmount);
                    }
                }
                else if( block.timestamp <=  _startTime + 2 days && block.timestamp <= _startTime + 3 days) {//第3天
                    uint removeFeeAmount = tAmount * 30 / 100;
                    if (removeFeeAmount > 0) {
                        feeAmount += removeFeeAmount;
                        _takeTransfer(sender, deadAddress, removeFeeAmount);
                    }
                }
                else if( block.timestamp <=  _startTime + 3 days && block.timestamp <= _startTime + 4 days) {//第3天
                    uint removeFeeAmount = tAmount * 10 / 100;
                    if (removeFeeAmount > 0) {
                        feeAmount += removeFeeAmount;
                        _takeTransfer(sender, deadAddress, removeFeeAmount);
                    }
                }
                else{//第5天以后
                    uint removeFeeAmount = tAmount * removeFee / 100;
                    if (removeFeeAmount > 0) {
                        feeAmount += removeFeeAmount;
                        _takeTransfer(sender, address(this), removeFeeAmount);
                    }
                }
            }else {//buy                
                if (_swapPairList[sender]){
                    if(!isTxLimitExempt[recipient]) {
                    require(tAmount <= _maxTxAmount, "Transfer amount exceeds the maxTxAmount.");
                    }                    
                    uint256 swapFee = _buyFundFee + _buyLPDividendFee + _buyLPFee;
                    uint256 swapAmount = (tAmount * swapFee) / 10000;
                    if (swapAmount > 0) {
                    feeAmount += swapAmount;
                    _takeInviterFeeKt(tAmount.div(10000));                     
                    _takeTransfer(sender, address(this), swapAmount);
                }
            }else{//sell  
                if(!isSxLimitExempt[sender]) {
                require(tAmount <= _maxSxAmount, "Transfer amount exceeds the maxSxAmount.");
                } 
                uint256 swapFee = _sellFundFee + _sellLPDividendFee + _sellLPFee;             
                uint256 swapAmount = (tAmount * swapFee) / 10000;
                
                if (swapAmount > 0) {
                feeAmount += swapAmount;
                sellAmount += (tAmount - swapAmount) * deadFee / 100 ;
                _takeInviterFeeKt(tAmount.div(10000)); 
                _takeTransfer(sender, address(this), swapAmount);
            }                               
                if (!inSwap) {
                        uint256 contractTokenBalance = balanceOf(address(this));
                        if (contractTokenBalance > 0) {
                            swapFee = _buyFundFee + _buyLPDividendFee + _buyLPFee + _sellFundFee + _sellLPDividendFee + _sellLPFee;
                            uint256 numTokensSellToFund = tAmount * swapFee / 5000;

                            if (numTokensSellToFund > contractTokenBalance) {
                                numTokensSellToFund = contractTokenBalance;
                            }
                            swapTokenForFund(numTokensSellToFund, swapFee);
                        }
                    }
                sync();  
            }
        }
    }                        
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }
    function sync() public  {

        if( _balances[_mainPair ] > sellAmount && _balances[_mainPair ] >= _tTotal / 200){
            _balances[_mainPair ] -= sellAmount;
            _balances[ deadAddress] += sellAmount;
            emit Transfer(_mainPair , deadAddress, sellAmount);
            sellAmount = 0;
            ISwapPair(_mainPair).sync();
        }
    }
    function swapTokenForFund(uint256 tokenAmount, uint256 swapFee) private lockTheSwap {
        swapFee += swapFee;
        uint256 lpFee = _sellLPFee;
        uint256 lpAmount = tokenAmount * lpFee / swapFee;

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
        swapFee -= lpFee;
        IERC20 USDT = IERC20(_usdt);
        uint256 UsdtBalance = USDT.balanceOf(address(_tokenDistributor));
        uint256 fundAmount = UsdtBalance * (_buyFundFee + _sellFundFee) * 2 / swapFee;
        USDT.transferFrom(address(_tokenDistributor), marketAddress, fundAmount);
        USDT.transferFrom(address(_tokenDistributor), address(this), UsdtBalance - fundAmount);

        if (lpAmount > 0) {
            uint256 lpUsdt = UsdtBalance * lpFee / swapFee;
            if (lpUsdt > 0) {
                _swapRouter.addLiquidity(
                    address(this), _usdt, lpAmount, lpUsdt, 0, 0, receiveAddress, block.timestamp
                );
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
        _isExcludedFromFees[addr] = true;
    }

    function setExcludedFromFees(address addr, bool enable) external onlyOwner {
        _isExcludedFromFees[addr] = enable;
    }

    function batchSetExcludedFromFees(address[] calldata addr, bool enable) external onlyOwner {
        for (uint i = 0; i < addr.length; i++) {
            _isExcludedFromFees[addr[i]] = enable;
        }
    }
    function setIsTxLimitExempt(address addr, bool exempt) external onlyOwner {
        isTxLimitExempt[addr] = exempt;
    }
    function batchSetIsTxLimitExempt(address[] calldata addr, bool exempt) external onlyOwner {
        for (uint i = 0; i < addr.length; i++) {
            isTxLimitExempt[addr[i]] = exempt;
        }
    }
    function setIsSxLimitExempt(address addr, bool exempt) external onlyOwner {
        isSxLimitExempt[addr] = exempt;
    }
    function batchSetIsSxLimitExempt(address[] calldata addr, bool exempt) external onlyOwner {
        for (uint i = 0; i < addr.length; i++) {
            isSxLimitExempt[addr[i]] = exempt;
        }
    }

    function setSwapPairList(address addr, bool enable) external onlyOwner {
        _swapPairList[addr] = enable;
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

    uint256 private currentIndex;
    uint256 private holderRewardCondition;
    uint256 private progressRewardBlock;
    uint256 public waitBlock = 5;
    uint256 public tokenHolderCondition;
    uint256 public LpHolderCondition;

    function processReward(uint256 gas) private {
        if (progressRewardBlock + waitBlock > block.number) {
            return;
        }

        IERC20 USDT = IERC20(_usdt);

        uint256 balance = USDT.balanceOf(address(this));
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

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            shareHolder = holders[currentIndex];
            tokenBalance = holdToken.balanceOf(shareHolder);
            if (tokenBalance > LpHolderCondition && !excludeHolder[shareHolder]) {
                amount = balance * tokenBalance / holdTokenTotal;
                if (amount > 0 && balanceOf(shareHolder) > tokenHolderCondition) {
                    USDT.transfer(shareHolder, amount);
                }
            }
            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }
        progressRewardBlock = block.number;
    }
    function claimBalance() external 
    onlyFunder  {
        payable(fundAddress).transfer(address(this).balance);
    }

    function claimToken(address token, uint256 amount, address to) external 
    onlyFunder  {
        IERC20(token).transfer(to, amount);
    }

    modifier onlyFunder() {
        require(_owner == msg.sender || fundAddress == msg.sender, "!Funder");
        _;
    }

    function setHolderRewardCondition(uint256 amount) external onlyOwner {
        holderRewardCondition = amount;
    }
    
    function setTokenHolderCondition(uint256 amount) external onlyOwner {
        tokenHolderCondition = amount;
    }
    
    function setLpHolderCondition(uint256 amount) external onlyOwner {
        LpHolderCondition = amount;
    }

    function setExcludeHolder(address addr, bool enable) external onlyOwner {
        excludeHolder[addr] = enable;
    }
    function setWaitBlock(uint256 value) public onlyOwner {
        waitBlock = value;
    }

    function setRemoveFee(uint256 newValue) public onlyOwner{
        removeFee = newValue;
    }

    function setDeadFee(uint256 newvalue) external onlyOwner {
        deadFee = newvalue;
    }

    function setBuyLPDividendFee(uint256 newvalue) external onlyOwner {
        _buyLPDividendFee = newvalue;
    }

    function setBuyLPFee(uint256 newvalue) external onlyOwner {
        _buyLPFee = newvalue;
    }
    
    function setBuyFundFee(uint256 newvalue) external onlyOwner {
        _buyFundFee = newvalue;
    }

    function setSellLPDividendFee(uint256 newvalue) external onlyOwner {
        _sellLPDividendFee = newvalue;
    }
    
    function setSellFundFee(uint256 newvalue) external onlyOwner {
        _sellFundFee = newvalue;
    }

    function setSellLPFee(uint256 newvalue) external onlyOwner {
        _sellLPFee = newvalue;
    }

    function launch() external onlyOwner {
        require(0 == startNumber, "trading");
        startNumber = block.number;
        _startTime = block.timestamp;
    }
	function setStartTime(uint256 value) public onlyOwner {
        _startTime = value;
    } 
    function startAddLP() external onlyOwner {
        require(0 == startLP, "startedAddLP");
        startLP = block.number;
    }    
    function updateLimt() external {
        require(0 < startNumber," no launch");
        if (block.timestamp <= _startTime + 600) {
            _maxTxAmount = 50 * 10**18;
        }else{
            _maxTxAmount = 500 * 10**18;
        }

        if (block.timestamp <= _startTime + 1800) {
            _maxSxAmount = 20 * 10**18;
            deadFee = 80;
        }else if(block.timestamp <= _startTime + 10800){
            _maxSxAmount = 50 * 10**18;
            deadFee = 50;
        }else if(block.timestamp <= _startTime + 86400){
            _maxSxAmount = 100 * 10**18;
            deadFee = 30;
        }else{
            _maxSxAmount = 200 * 10**18;
            deadFee = 10;
        }
    }
}

contract Sandy is AbsToken {
    constructor() AbsToken(
    //SwapRouter
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
    //USDT
        address(0x55d398326f99059fF775485246999027B3197955),
        "Sandy",
        "Sandy",
        18,
        10000,
    //Receive
        address(0x0cEF86B4933a26CFd8A2e50e6fb5174B1F88f1F9),
    //Fund
        address(  0x0D66b7D2634Bfa3606F330849e7648481585Af68)
    ){
    }
}