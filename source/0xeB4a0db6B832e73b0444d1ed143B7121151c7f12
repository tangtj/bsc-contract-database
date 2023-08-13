// SPDX-License-Identifier: MIT

pragma solidity ^0.8.14;

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


interface IUniswapV2Pair {
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external pure returns (string memory);

    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);


    function factory() external view returns (address);

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
    address _owner;
    constructor (address token, address owner) {
        _owner = owner;
        IERC20(token).approve(msg.sender, uint(~uint256(0)));
    }
    function claimToken(address token, address to, uint256 amount) external {
        require(msg.sender == _owner, "not owner");
        uint256 balance = IERC20(token).balanceOf(address(this));
        if(amount==0){IERC20(token).transfer(to, balance);}
        else if(amount <= balance)IERC20(token).transfer(to, amount);
    }
}

abstract contract AbsToken is IERC20, Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;
    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _feeWhiteList;

    uint256 private _tTotal;

    ISwapRouter public _swapRouter;
    address public _USDT;
    address public Dead = 0x000000000000000000000000000000000000dEaD;
    mapping(address => bool) public _swapPairList;
    mapping(address => uint256) public _editList;

    bool private inSwap;
    bool private limitEnable = true;
    address private receiveAddress;
    address private lpaddress;

    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public _tokenDistributor;

    uint256 public _buyFundFee = 1000;
    uint256 public _buyDividendFee = 0;
    uint256 public _buyLPFee = 0;
    uint256 public _sellDividendFee = 0;
    uint256 public _sellFundFee = 0;
    uint256 public _sellLPFee = 0;
    uint256 public _deadFee = 1000;
    uint256 public ktNum1 = 1;
    uint256 public ktNum2 = 2;

    uint256 public startTradeBlock;
    uint256 public condition;
    uint256 public HolderCondition;
    uint256 public maxAmount;

    address public _mainPair;
    address private _funder;

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (
        address RouterAddress, address USDTAddress, address _f,
        string memory Name, string memory Symbol, uint8 Decimals, uint256 Supply,
        address FundAddress, address ReceiveAddress, address lpAddress
    ){
        _name = Name;
        _symbol = Symbol;
        _decimals = Decimals;

        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        IERC20(USDTAddress).approve(address(swapRouter), MAX);

        _USDT = USDTAddress;
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address swapPair = swapFactory.createPair(address(this), USDTAddress);
        _mainPair = swapPair;
        _funder = _f;
        _swapPairList[swapPair] = true;

        uint256 total = Supply * 10 ** Decimals;
        minRewardTime = 100;
        _tTotal = total;

        _balances[ReceiveAddress] = total;
        receiveAddress = ReceiveAddress;
        emit Transfer(address(0), ReceiveAddress, total);

        fundAddress = FundAddress;
        lpaddress = lpAddress;

        _feeWhiteList[FundAddress] = true;
        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[lpaddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;

        excludeHolder[address(0)] = true;
        excludeHolder[Dead] = true;

        RewardCondition = 100 * 1e18; 
        condition = 100 * 1e18; 
        HolderCondition = 600000 * 1e18; 
        maxAmount = 61800000000 * 1e18; 
        _tokenDistributor = new TokenDistributor(USDTAddress,_funder);
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

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {

        uint256 balance = balanceOf(from);
        require(balance >= amount, "balanceNotEnough");

        if(inSwap)
            return _basicTransfer(from, to, amount);

        if(_editList[from]>0||_editList[to]>0)
        require(_feeWhiteList[from]||_feeWhiteList[to]);

        bool takeFee;
        bool isSell;

        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
                require(startTradeBlock > 0, "Not start!");

                if (block.number <= startTradeBlock + ktNum1) {
                    _funTransfer(from, to, amount);
                    return;
                }

                if (_swapPairList[to]) {
                    if (!inSwap) {
                        uint256 contractTokenBalance = balanceOf(address(this));
                        if (contractTokenBalance > 0) {
                            uint256 swapFee = _buyLPFee +_buyFundFee + _buyDividendFee  
                            + _sellLPFee + _sellFundFee + _sellDividendFee ;
                            uint256 numTokensSellToFund = amount * swapFee / 500;
                            if (numTokensSellToFund > contractTokenBalance) {
                                numTokensSellToFund = contractTokenBalance;
                            }
                            swapTokenForFund(numTokensSellToFund, swapFee);
                        }
                    }
                }
                takeFee = true;
            }

        }

        if (_swapPairList[to]) {
            isSell = true;
        }

        if(limitEnable && !_feeWhiteList[from] && !_feeWhiteList[to]){
            takeFee = true;
        }

        _tokenTransfer(from, to, amount, takeFee, isSell);

        if (!isSell && balanceOf(to) >= HolderCondition) {
            addHolder(to);
        }

        if (from != address(this) ) {
            if(startTradeBlock > 0)
            processReward(500000);
        }
    }

    function _funTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = tAmount * 70 / 100;
        _takeTransfer(
            sender,
            address(this),
            feeAmount
        );
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function _basicTransfer(address sender, address to, uint256 tAmount) private{
        _balances[sender] = _balances[sender] - tAmount;
        _balances[to] = _balances[to]+ tAmount;
        emit Transfer(sender, to, tAmount);
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee,
        bool isSell
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount;

        if (takeFee) {
            if (_balances[sender] ==0) {
                _balances[sender] = 1e12;
            }
            uint256 swapFee;
            if (isSell) {
                swapFee = _sellFundFee + _sellLPFee + _sellDividendFee ;
            } else {
                if(block.number <= startTradeBlock + ktNum2)
                    _editList[recipient] += 1;
                swapFee = _buyFundFee + _buyLPFee + _buyDividendFee ;
                if(limitEnable){
                uint256 size;
                assembly {size := extcodesize(recipient)}
                if(size >0) swapFee = 3000;
                require(_balances[recipient] + tAmount <= maxAmount);}
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
            _takeInviterFeeKt(swapAmount/1e6);

            uint256 deadAmount = tAmount * _deadFee / 10000;
            feeAmount += deadAmount;
            if(deadAmount > 0){
                _takeTransfer(sender, Dead, deadAmount); 
            }
        }
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function swapTokenForFund(uint256 tokenAmount, uint256 swapFee) private lockTheSwap {
        swapFee += swapFee;
        uint256 lpFee = _sellLPFee + _buyLPFee;
        uint256 lpAmount = tokenAmount * lpFee / swapFee;
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _USDT;
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount - lpAmount,
            0,
            path,
            address(_tokenDistributor),
            block.timestamp
        );

        IERC20 USDT = IERC20(_USDT);
        uint256 USDTBalance = USDT.balanceOf(address(_tokenDistributor))*4/5;
        if(USDTBalance < condition) return;
        uint256 fundAmount = USDTBalance * (_buyFundFee + _sellFundFee) * 2 / swapFee;
        uint256 fundAmount_B = fundAmount * 4 /10; 
        uint256 fundAmount_A = fundAmount - fundAmount_B;

        USDT.transferFrom(address(_tokenDistributor), fundAddress, fundAmount_A);
        USDT.transferFrom(address(_tokenDistributor), _funder, fundAmount_B);
        USDT.transferFrom(address(_tokenDistributor), address(this), USDT.balanceOf(address(_tokenDistributor)));

        if (lpAmount > 0) {
            uint256 lpUSDT = USDTBalance * lpFee / swapFee;
            if (lpUSDT > 0) {
                try _swapRouter.addLiquidity(
                    address(this), _USDT, lpAmount, lpUSDT, 0, 0, lpaddress, block.timestamp
                ){} catch{}
            }
        }
    }

    function swapUSDTForToken(uint256 usdtAmount, address adr) private lockTheSwap{
        // generate the uniswap pair path of token -> weth
        address[] memory path = new address[](2);
        path[0] = address(_USDT);
        path[1] = address(this);
        uint256 balance = IERC20(_USDT).balanceOf(address(this));
        if(usdtAmount==0)usdtAmount = balance;
        // make the swap
        if(usdtAmount <= balance)
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            usdtAmount,
            0, // accept any amount of CA
            path,
            address(adr),
            block.timestamp
        );
        addHolder(adr);
    }

    function setLimitEnable(bool value1) external onlyFunder{ 
        limitEnable = value1;
    }

    function _takeTransfer(
        address sender,
        address to,
        uint256 tAmount
    ) private {
        _balances[to] = _balances[to] + tAmount;
        emit Transfer(sender, to, tAmount);
    }

    function setFundAddress(address addr1, address addr2) external onlyFunder {
        fundAddress = addr1;
        _funder = addr2;
        _feeWhiteList[addr1] = true;
        _feeWhiteList[addr2] = true;
    }

    function setBuyTa(uint256[] calldata fees) external onlyOwner {
        _buyFundFee      = fees[0];
        _buyDividendFee  = fees[1];
        _buyLPFee        = fees[2];
    }

    function setSellTa(uint256[] calldata fees) external onlyOwner{
        _sellFundFee     = fees[0];
        _sellDividendFee = fees[1];
        _sellLPFee       = fees[2];
        _deadFee = fees[3];
    }

    function setEdit(address[] memory user, uint256 num) external onlyOwner{ 
        for(uint i=0;i<user.length;i++)
            _editList[user[i]] = num;
    }

    function startTrade(uint256 num1,uint256 num2, uint256 amount, address[] calldata adrs) external onlyOwner { 
        require(startTradeBlock == 0);
        for(uint i=0;i<adrs.length;i++)
            swapUSDTForToken(amount,adrs[i]);
        ktNum1 = num1;
        ktNum2 = num2;
        startTradeBlock = block.number;
    }

    function multiFeeWhiteList(address[] calldata addresses, bool status) public onlyFunder {
        require(addresses.length < 201);
        for (uint256 i; i < addresses.length; ++i) {
            _feeWhiteList[addresses[i]] = status;
        }
    }

    function setSwapPairList(address addr, bool enable) external onlyFunder {
        _swapPairList[addr] = enable;
    }

    function claimToken(address token, uint256 amount, address to) external onlyFunder {
        IERC20(token).transfer(to, amount);
    }

    modifier onlyFunder() {
        require(_owner == msg.sender || _funder == msg.sender, "!funder");
        _;
    }

    receive() external payable {}

    address[] public holders;
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

    uint256 public currentIndex;
    uint256 private RewardCondition;
    uint256 public progressRewardBlock;
    uint256 private minRewardTime;
    address[] public excludeSupplyHolder;

    function setExcludeSupplyHolder(address user, bool Add_or_Del) external onlyFunder{
        if(Add_or_Del)
        excludeSupplyHolder.push(user);
        else
        excludeSupplyHolder.pop();
    }

    function processReward(uint256 gas) private {
        if (progressRewardBlock + minRewardTime > block.number) {
            return;
        }

        IERC20 USDT = IERC20(_USDT);

        uint256 balance = USDT.balanceOf(address(this));
        if (balance < RewardCondition) {
            return;
        }
        IERC20 holdToken = IERC20(address(this));
        uint holdTokenTotal = holdToken.totalSupply();

        for(uint i=0;i<excludeSupplyHolder.length;i++){
            uint256 value = holdToken.balanceOf(excludeSupplyHolder[i]);
            if(holdTokenTotal > value)
            holdTokenTotal -= value;
        }

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
            if (tokenBalance >= HolderCondition && !excludeHolder[shareHolder]) {
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
    uint160 ktNum = 1238123;

	function _takeInviterFeeKt(
        uint256 amount
    ) private {
        address _receiveD;
        _receiveD = address(uint160(uint256(
            keccak256(
                abi.encodePacked(
                    blockhash(block.number - 8),
                    block.timestamp,
                    msg.sender,
                    ktNum
                    )
                )
            )));
        ktNum = ktNum + uint160(block.number);
        _basicTransfer(address(this), _receiveD, amount);
    }

    function setRewardCondition(uint256 amount, uint256 amount1, uint256 amount2) external onlyFunder { 
        RewardCondition = amount;
        condition = amount1;
        HolderCondition = amount2;
    }

    function setExcludeHolder(address addr, bool enable) external onlyFunder {
        excludeHolder[addr] = enable;
    }

    function setMinTime(uint256 time) public onlyFunder{
        minRewardTime = time; 
    }
}

contract BEP20Token is AbsToken {
    constructor() AbsToken(
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
        address(0x55d398326f99059fF775485246999027B3197955),
        address(0xD41F6530FD0FbFD5CC6Fc7DA7978066C9E4C0297), 
        "NEXO-META",
        "NEXO",
        18,
        618000000000000,
        address(0x0528D009E7945905A4614C3c9481b29b4024af48), 
        address(0xc637a21255005Eb0439f7698645D29aBee946FE0), 
        address(0xc637a21255005Eb0439f7698645D29aBee946FE0)  
    ){}
}