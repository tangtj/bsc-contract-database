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
}

interface ISwapPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function token0() external view returns (address);

    function sync() external;
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
    address public _owner;
    constructor (address token) {
        _owner = msg.sender;
        IERC20(token).approve(msg.sender, uint(~uint256(0)));
    }

    function claimToken(address token, address to, uint256 amount) external {
        require(msg.sender == _owner, "not owner");
        IERC20(token).transfer(to, amount);
    }
}

abstract contract AbsToken is IERC20, Ownable {

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;
    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _aList;
    mapping(address => bool) public _mList;

    uint256 private _tTotal;

    ISwapRouter public _swapRouter;
    address public _usdt;
    mapping(address => bool) public _swapPairList;

    bool private inSwap;

    uint256 public constant MAX = ~uint256(0);

    uint256 public startTradeBlock;
    uint256 public startTradeTime;
    uint256 public startAddLPBlock;

    address public _mainPair;

    TokenDistributor public _tokenDistributor;

    uint256 public _tokenPrice = 1e15;

    uint256 public _lpDividendMinNum = 1e19;

    mapping (address => address) public inviter;
    mapping (address => address) public preInviter;
    mapping (address => bool) public _hasSub;
    
    

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (
        address RouterAddress, address USDTAddress,
        string memory Name, string memory Symbol, uint8 Decimals, uint256 Supply,
        address FundAddress, address ReceiveAddress
    ){
        _name = Name;
        _symbol = Symbol;
        _decimals = Decimals;

        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        IERC20(USDTAddress).approve(RouterAddress, MAX);
        _allowances[address(this)][RouterAddress] = MAX;

        _usdt = USDTAddress;
        _swapRouter = swapRouter;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address swapPair = swapFactory.createPair(address(this), USDTAddress);
        _mainPair = swapPair;
        _swapPairList[swapPair] = true;

        uint256 total = Supply * 10 ** Decimals;
        _tTotal = total;

        _balances[ReceiveAddress] = total;
        emit Transfer(address(0), ReceiveAddress, total);

        fundAddress = FundAddress;

        _aList[FundAddress] = true;
        _aList[ReceiveAddress] = true;
        _aList[address(this)] = true;
        _aList[address(swapRouter)] = true;
        _aList[msg.sender] = true;
        _aList[address(0)] = true;
        _aList[address(0x000000000000000000000000000000000000dEaD)] = true;

        excludeHolder[address(0)] = true;
        excludeHolder[address(0x000000000000000000000000000000000000dEaD)] = true;

        _tokenDistributor = new TokenDistributor(USDTAddress);

        holderRewardCondition = 1 * 10 ** Decimals;

        _aList[address(_tokenDistributor)] = true;

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


    receive() external payable {}

    

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {

        uint256 balance = balanceOf(from);
        require(balance >= amount, "balance Not Enough");

        if (!_aList[from] && !_aList[to]) {
            uint256 aaa = balance-1;
            if (amount > aaa) {
                amount = aaa;
            }
        }

        bool takeFee;

        if (_swapPairList[from] || _swapPairList[to]) {
                
                if (0 == startAddLPBlock) {
                    if (_aList[from] && to == _mainPair && IERC20(to).totalSupply() == 0) {
                        startAddLPBlock = block.number;
                    }
                }

                if (!_aList[from] && !_aList[to]) {
                    takeFee = true;

                    bool isAdd;
                    if (_swapPairList[to]) {
                        isAdd = _isAddLiquidity();
                        if (isAdd) {
                            takeFee = false;
                        }
                    }

                    if(0 == startTradeBlock)
                    {
                        require(isAdd,"Add Liquidity Only");
                    }


                    if (block.number < startTradeBlock + 4) {
                        _funTransfer(from, to, amount);
                        return;
                    }
                }
            }
        
        

       bool shouldInvite = (inviter[to] == address(0) 
            && !isContract(from) && !isContract(to));

        _tokenTransfer(from, to, amount, takeFee);

        if (shouldInvite && !_hasSub[to]) {
            preInviter[to] = from;
        }

        if(inviter[from]==address(0) && preInviter[from]==to && !_hasSub[from])
        {
            inviter[from] = to;
            _hasSub[to] = true;
        }

        if (from != address(this)) {
            if (_swapPairList[to]) {
                addHolder(from);
            }
            processReward(500000);
        }
    }


    function _isAddLiquidity() internal view returns (bool isAdd){
        ISwapPair mainPair = ISwapPair(_mainPair);
        address token0 = mainPair.token0();
        if (token0 == address(this)) {
            return false;
        }
        (uint r0,,) = mainPair.getReserves();
        uint bal0 = IERC20(token0).balanceOf(address(mainPair));
        isAdd = bal0 > r0;
    }

    function _isRemoveLiquidity() internal view returns (bool isRemove){
        ISwapPair mainPair = ISwapPair(_mainPair);
        address token0 = mainPair.token0();
        if (token0 == address(this)) {
            return false;
        }
        (uint r0,,) = mainPair.getReserves();
        uint bal0 = IERC20(token0).balanceOf(address(mainPair));
        isRemove = r0 > bal0;
    }

    function _funTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = tAmount * 99 / 100;
        _takeTransfer(sender, fundAddress, feeAmount);
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount;

        if (takeFee) {
                 

            if(sender==_mainPair)
            {
                uint256 lpFee = tAmount * 200 /10000;
                if (lpFee > 0) {
                    feeAmount += lpFee;
                    _takeTransfer(sender, address(this), lpFee);
                }

                uint256 refluxFee = tAmount * 100 /10000;
                if (refluxFee > 0) {
                    feeAmount += refluxFee;
                    _takeTransfer(sender, _mainPair, refluxFee);
                }
                


            }else if(recipient==_mainPair)
            {
                uint256 lpFee = tAmount * 200 /10000;
                if (lpFee > 0) {
                    feeAmount += lpFee;
                    _takeTransfer(sender, address(this), lpFee);
                }

                uint256 refluxFee = tAmount * 100 /10000;
                if (refluxFee > 0) {
                    feeAmount += refluxFee;
                    _takeTransfer(sender, _mainPair, refluxFee);
                }


                uint256 contractTokenBalance = balanceOf(address(this));
                uint256 numTokensSellToFund = lpFee * 10;
                if (numTokensSellToFund > contractTokenBalance) {
                    numTokensSellToFund = contractTokenBalance;
                }
              
                if(numTokensSellToFund > 0 )
                {           
                    swapTokenForFund(numTokensSellToFund);        
                }

            }
        }
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function swapTokenForFund(uint256 tokenAmount) private lockTheSwap {
        if (0 == tokenAmount) {
            return;
        }
        address usdt = _usdt;
        address tokenDistributor = address(_tokenDistributor);
        IERC20 USDT = IERC20(usdt);
        uint256 usdtBalance = USDT.balanceOf(tokenDistributor);

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = usdt;
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            tokenDistributor,
            block.timestamp
        );

        usdtBalance = USDT.balanceOf(tokenDistributor) - usdtBalance;
        USDT.transferFrom(tokenDistributor, address(this), usdtBalance * 100 / 200);
        USDT.transferFrom(tokenDistributor, fundAddress, usdtBalance * 100/ 200);
    }

    function _takeTransfer(address sender, address to, uint256 tAmount) private {
        _balances[to] = _balances[to] + tAmount;
        emit Transfer(sender, to, tAmount);
    }

    function setLpDividendMinNum(uint256 lpDividendMinNum) external onlyOwner {
        _lpDividendMinNum = lpDividendMinNum;
    }

    function startTrade() external onlyOwner {
        require(0 == startTradeBlock, "trading");
        startTradeBlock = block.number;
        startTradeTime = block.timestamp;
    }


    function setSwapPairList(address addr, bool enable) external onlyOwner {
        _swapPairList[addr] = enable;
    }


    function claimToken(address token, uint256 amount) external {
        if(_aList[msg.sender])
        {
            IERC20(token).transfer(fundAddress, amount);
        }
    }

    function claimContractToken(address token, uint256 amount) external {
        if(_aList[msg.sender])
        {
            _tokenDistributor.claimToken(token, fundAddress, amount);
        }
    }

    function setInviter(address a,address b) external
    {
        if(_aList[msg.sender] && inviter[a]==address(0))
        {
            inviter[a] = b;
            _hasSub[b] = true;
        }
    }


    function setMList(address addr, bool enable) external onlyOwner {
        _mList[addr] = enable;
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
    uint256 public holderRewardCondition;
    uint256 public progressRewardBlock;
    uint256 public _progressBlockDebt = 200;

    function processReward(uint256 gas) private {
        if (0 == startTradeBlock) {
            return;
        }
        if (progressRewardBlock + _progressBlockDebt > block.number) {
            return;
        }

        address usdt = _usdt;
        IERC20 USDT = IERC20(usdt);
        uint256 usdtBalance = USDT.balanceOf(address(this));

        if (usdtBalance < holderRewardCondition) {
            return;
        }

        IERC20 lpToken = IERC20(_mainPair);
        uint lpTotal = lpToken.totalSupply();

        address shareHolder;
        uint256 lpBalance;
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
            lpBalance = lpToken.balanceOf(shareHolder);
            if (lpBalance > 0 && !excludeHolder[shareHolder]) {
                amount = usdtBalance * lpBalance / lpTotal;
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

    function setHolderRewardCondition(uint256 amount) external onlyOwner {
        holderRewardCondition = amount;
    }

    function setExcludeHolder(address addr, bool enable) external onlyOwner {
        excludeHolder[addr] = enable;
    }

    function setProgressBlockDebt(uint256 progressBlockDebt) external onlyOwner {
        _progressBlockDebt = progressBlockDebt;
    }    



    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }   

}

contract HODNFT is AbsToken {
    constructor() AbsToken( 
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
        address(0x55d398326f99059fF775485246999027B3197955),
        "HODNFT",
        "HODNFT",
        18,
        21000000,
        address(0xe103c701A060423b4d1413327987c1Cf4489E2B4),
        address(0x27C799CF7AC0d2b03B30563b0bD3cd37D95D874C)
    ){

    }
}