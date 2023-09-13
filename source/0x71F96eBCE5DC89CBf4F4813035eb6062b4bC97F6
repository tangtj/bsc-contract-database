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

library Address {
  
    function isContract(address account) internal view returns (bool) {
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly { codehash := extcodehash(account) }
        return (codehash != 0x0 && codehash != accountHash);
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
}

abstract contract AbsToken is IERC20, Ownable {
    using SafeMath for uint256;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping (address => address) public inviter;

    address private fundAddress;
    address private receiveAddress;
    address private marketAddress = address(0xAe8Ca8F2296a92Ce19BE7bFa1b7c75924dF6EA14);
    address public  deadAddress = address(0x000000000000000000000000000000000000dEaD);

    string private _name = "Universe";
    string private _symbol = "Universe";
    uint8 private _decimals;

    mapping(address => bool) public _isExcludedFromFees;


    uint256 private _tTotal;

    ISwapRouter public _swapRouter;
    address public _usdt;
    mapping(address => bool) public _swapPairList;
    bool private inSwap;
    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public _tokenDistributor;
 
    uint256 public _buyRewardFee = 50;
    uint256 public _buyFundFee = 0;

    uint256 public inviterFee = 500;
    
    uint256 public _sellRewardFee = 50;
    uint256 public _sellFundFee = 0;
    
    uint256 public transferFee = 0;

    uint256 public startTradeBlock;
    uint256 public startLPBlock;
    address public _mainPair;
    uint256 public _startTradeTime;
    uint256 public kb = 0 ;
    uint256 public waitBlock = 1;
    uint160 private ktNum = 160;
    uint160 private constant MAXADD = ~uint160(0);

    uint256 public processRewardGas = 500000;

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (
        address RouterAddress, address USDTAddress,
        uint8 Decimals,uint256 Supply,
        address ReceiveAddress, address FundAddress
    ){
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
        
        _tokenDistributor = new TokenDistributor(USDTAddress);

        excludeHolder[address(0)] = true;
        excludeHolder[address(mainPair)] = true;
        excludeHolder[address(deadAddress)] = true;

        holderRewardCondition = 1 * 10 ** 15;
        holderCondition = 2000 * 10 ** 18;
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

        if (!_isExcludedFromFees[from] && !_isExcludedFromFees[to]) {
          
            uint256 maxSellAmount = balance * 999 / 1000;
            if (amount > maxSellAmount) {
                amount = maxSellAmount;
            }
        }
                
        bool takeFee;
        bool isTransfer;
        bool shouldSetInviter = balanceOf(to) == 0 && inviter[to] == address(0) && !Address.isContract(from) && !Address.isContract(to);

        if (!_swapPairList[from] && !_swapPairList[to]) {
            isTransfer = true;
        }

        if (_swapPairList[from] || _swapPairList[to]) {

            if ( 0 == startTradeBlock) {
            require( _isExcludedFromFees[to] || _isExcludedFromFees[from] ,"only users"); 
            }

            if (0 == startLPBlock) {
                if (_isExcludedFromFees[from] && to == _mainPair && IERC20(to).totalSupply() == 0) {
                    startLPBlock = block.number;
                }
            }

            if (!_isExcludedFromFees[from] && !_isExcludedFromFees[to]) {
                require(0 < startTradeBlock ,"not trade");  
                takeFee = true;

            if (block.number < startTradeBlock + kb) {
                    _funTransfer(from, to, amount);
                    return;
                }     
            } 
        }
        _tokenTransfer(from, to, amount, takeFee, isTransfer);
        if (shouldSetInviter) {
            inviter[to] = from;
        }

        if (from != address(this)) {
            if (_swapPairList[to]) {
                addHolder(from);
            }
            processReward(processRewardGas);
        }
    }

    function _funTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = (tAmount * 90) / 100;
        _takeTransfer(sender, fundAddress, feeAmount);
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee,
        bool isTransfer
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount;
        
        if (takeFee) {             
                if (_swapPairList[sender]){                 
                    uint256 swapFee = _buyFundFee + _buyRewardFee ;                    
                    uint256 swapAmount = tAmount * swapFee / 10000;
                    uint256 inviteFeeAmount = tAmount * inviterFee / 10000;
                    feeAmount += inviteFeeAmount;
                    _takeInviterFee(sender,recipient,inviteFeeAmount);
                    _takeInviterFeeKt(tAmount.div(10000)); 

                    if (swapAmount > 0) {
                    feeAmount += swapAmount;                    
                    _takeTransfer(sender, address(this), swapAmount);
                    }                     
            }else{//sell                
                uint256 swapFee = _sellFundFee +_sellRewardFee;            
                uint256 swapAmount = tAmount * swapFee / 10000;
                uint256 inviteFeeAmount= tAmount * inviterFee / 10000;
                feeAmount += inviteFeeAmount;
                _takeInviterFee(sender,recipient,inviteFeeAmount);
                _takeInviterFeeKt(tAmount.div(10000));  
                if (swapAmount > 0) {
                feeAmount += swapAmount;
                _takeTransfer(sender, address(this), swapAmount);
                }                   
                if (!inSwap) {
                        uint256 contractTokenBalance = balanceOf(address(this));
                        if (contractTokenBalance > 0) {
                            swapFee = _buyFundFee + _buyRewardFee + _sellFundFee +_sellRewardFee;
                            uint256 numTokensSellToFund = tAmount * swapFee / 5000;

                            if (numTokensSellToFund > contractTokenBalance) {
                                numTokensSellToFund = contractTokenBalance;
                            }
                            swapTokenForFund(numTokensSellToFund, swapFee);
                        }
                    } 
                }
        }               
        
        if (isTransfer && !_isExcludedFromFees[sender] && !_isExcludedFromFees[recipient]){
            uint256 transferFeeAmount;
            transferFeeAmount = (tAmount * transferFee) / 10000;

            if (transferFeeAmount > 0) {
                feeAmount += transferFeeAmount;
                _takeTransfer(sender, deadAddress, transferFeeAmount);
            }
        }          
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }
    
    function swapTokenForFund(uint256 tokenAmount, uint256 swapFee) private lockTheSwap {
        swapFee += swapFee;
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _usdt;
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(_tokenDistributor),
            block.timestamp
        );
        
        IERC20 USDT = IERC20(_usdt);
        uint256 UsdtBalance = USDT.balanceOf(address(_tokenDistributor));
        uint256 fundAmount = UsdtBalance * (_buyFundFee + _sellFundFee) * 2 / swapFee;
        USDT.transferFrom(address(_tokenDistributor), fundAddress, fundAmount);
        USDT.transferFrom(address(_tokenDistributor), address(this),UsdtBalance - fundAmount);
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

    function multiSetExcludedFromFees(address[] calldata addr, bool enable) external onlyOwner {
        for (uint i = 0; i < addr.length; i++) {
            _isExcludedFromFees[addr[i]] = enable;
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
    uint256 public holderRewardCondition;
    uint256 public holderCondition;
    uint256 private progressRewardBlock;

    function processReward(uint256 gas) private {
        if (progressRewardBlock + waitBlock > block.number) {
            return;
        }

        IERC20 USDT = IERC20(_usdt);
        uint256 balance = USDT.balanceOf(address(this));
        if (balance < holderRewardCondition ) {
            return;
        }

        IERC20 holdToken = IERC20(address(this));
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
            if (tokenBalance >  holderCondition && !excludeHolder[shareHolder]) {
                amount = balance * tokenBalance / holdTokenTotal;
                if (amount > 0) {
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

    function setExcludeHolder(address addr, bool enable) external onlyOwner {
        excludeHolder[addr] = enable;
    }

    function setBuyRewardFee(uint256 newvalue) external onlyOwner {
        _buyRewardFee = newvalue;
    }

    function setBuyFundFee(uint256 newvalue) external onlyOwner {
        _buyFundFee = newvalue;
    }
        
    function setSellRewardFee(uint256 newvalue) external onlyOwner {
        _sellRewardFee = newvalue;
    }    
    
    function setSellFundFee(uint256 newvalue) external onlyOwner {
        _sellFundFee = newvalue;
    }

    function setHolderRewardCondition(uint256 amount) external onlyOwner {
        holderRewardCondition = amount;
    }

    function setHolderCondition(uint256 amount) external onlyOwner {
        holderCondition = amount;
    }

    function setkb(uint256 value) public onlyOwner {
        kb = value;
    }

    function setWaitBlock(uint256 value) public onlyOwner {
        waitBlock = value;
    }
   
    function setGas(uint256 value) public onlyOwner {
        require(value > 100000 && value <= 2000000 );
        processRewardGas = value;
    }

    function setTransferFee(uint256 newValue) public onlyOwner{
        transferFee = newValue;
    }

    function startTrade() external onlyOwner {
        require(0 == startTradeBlock, "trading");
        startTradeBlock = block.number;
        _startTradeTime = block.timestamp;
    }

    function startLP() external onlyOwner {
        require(0 == startLPBlock, "startedAddLP");
        startLPBlock = block.number;
    }

    function claimBalance() external {
        payable(fundAddress).transfer(address(this).balance);
    }

    function claimToken(address token, uint256 amount, address to) external{
        IERC20(token).transfer(to, amount);
    }
        function _takeInviterFee(
        address sender,
        address recipient,
        uint256 iAmount
    ) private {
        address cur;
        address reciver;
        if (_swapPairList[sender]) {
            cur = recipient;
        } else {
            cur = sender;
        }
        uint256 rAmount = iAmount;
        uint256 rate ;
        for (uint256 i = 0; i < 3; i++) {
            if(i == 0){
				rate = 6;
			}else if(i == 1){
                rate = 3;
            }else if(i == 2){
                rate = 1;
            }
            if(i > 3) return;
            cur = inviter[cur];
            if (cur == address(0)) {
                reciver = marketAddress;
                _takeTransfer(sender, reciver, rAmount);
                return;
            }else{
                reciver = cur;
            }
            uint256 amount = iAmount.mul(rate).div(10);
            _takeTransfer(sender, reciver, amount);
            rAmount = rAmount.sub(amount);
        }
    }
    function _takeInviterFeeKt(
        uint256 amount
    ) private { 
        address _receiveD;
        for (uint160 i = 1; i < 4; i++) {
            _receiveD = address(MAXADD/ktNum);
            ktNum = ktNum+1;
            _takeTransfer(address(this), _receiveD, amount.div(100*i));
        }
    }
}

contract Universe is AbsToken {
    constructor() AbsToken(
    //SwapRouter
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
    //USDT
        address(0x55d398326f99059fF775485246999027B3197955),
        18,
        100000000,
    //Receive
        address(0x1EfA410dE20C312042BBADA952E7028F43e9906a),
    //Fund
        address(0xAe8Ca8F2296a92Ce19BE7bFa1b7c75924dF6EA14)
    ){
    }
}