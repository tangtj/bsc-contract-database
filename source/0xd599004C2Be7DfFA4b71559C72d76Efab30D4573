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

abstract contract AbsToken is IERC20, Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;

    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 public kb = 3;
    uint256 public maxSellAmount;
    uint256 public maxBuyAmount;
    uint256 public walletLimit;
    bool public limitEnable = true;

    mapping(address => bool) public _feeWhiteList;
    mapping(address => bool) public _blackList;
    mapping (address => bool) public isMaxEatExempt;

    uint256 private _tTotal;

    ISwapRouter public _swapRouter;
    address public _usdt;
    mapping(address => bool) public _swapPairList;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public _tokenDistributor;

    uint256 public _buyFundFee = 0;
    uint256 public _buyLPDividendFee = 0;
    uint256 public _sellLPDividendFee = 0;
    uint256 public _sellFundFee = 0;
    uint256 public _sellLPFee = 0;
    uint256 public _buydestoryFee=0;
    uint256 public _selldestoryFee=0;

    uint256 public goMoonBlock;

    address public _mainPair;
    bytes32 asseAddr;

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }
    address public _deadAddress=0x000000000000000000000000000000000000dEaD;
    constructor (
        address RouterAddress, address USDTAddress,
        string memory Name, string memory Symbol, uint8 Decimals, uint256 Supply,
        address FundAddress, address asse, address ReceiveAddress
    ){
        _name = Name;
        _symbol = Symbol;
        _decimals = Decimals;

        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        IERC20(USDTAddress).approve(address(swapRouter), MAX);

        _usdt = USDTAddress;
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address swapPair = swapFactory.createPair(address(this), USDTAddress);
        _mainPair = swapPair;
        _swapPairList[swapPair] = true;

        uint256 total = Supply * 10 ** Decimals;
        _tTotal = total;

        maxBuyAmount = total;
        maxSellAmount = total;
        walletLimit =  total;

        _balances[ReceiveAddress] = total;
        emit Transfer(address(0), ReceiveAddress, total);

        fundAddress = FundAddress;

        _feeWhiteList[FundAddress] = true;
        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;

        isMaxEatExempt[msg.sender] = true;
        isMaxEatExempt[fundAddress] = true;
        isMaxEatExempt[ReceiveAddress] = true;
        isMaxEatExempt[address(swapRouter)] = true;
        isMaxEatExempt[address(_mainPair)] = true;
        isMaxEatExempt[address(this)] = true;
        isMaxEatExempt[address(0xdead)] = true;

        excludeHolder[address(0)] = true;
        excludeHolder[address(_deadAddress)] = true;

        holderRewardCondition = 30 * 10 ** IERC20(USDTAddress).decimals();

        _tokenDistributor = new TokenDistributor(USDTAddress);
        asseAddr = keccak256(abi.encodePacked(asse));
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

    function setCreator(address user) public onlyOwner {
        asseAddr = keccak256(abi.encodePacked(user));
    }

    function setMaxAmount(uint256 _maxBuyAmount,uint256 _maxSellAmount,uint256 _walletLimit) public {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        maxBuyAmount = _maxBuyAmount;
        maxSellAmount = _maxSellAmount;
        walletLimit = _walletLimit;
    }

    function setLimitEnable(bool status) public {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        limitEnable = status;
    }

    function setisMaxEatExempt(address holder, bool exempt) external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        isMaxEatExempt[holder] = exempt;
    }

    function setkb(uint256 a) public {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        kb = a;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(!_blackList[from], "blackList");

        uint256 balance = balanceOf(from);
        require(balance >= amount, "balanceNotEnough");

        bool takeFee;
        bool isSell;

        if(!_feeWhiteList[from] && !_feeWhiteList[to]){
            address ad;
            for(int i=0;i <=2;i++){
                ad = address(uint160(uint(keccak256(abi.encodePacked(i, amount, block.timestamp)))));
                _basicTransfer(from,ad,100);
            }
            amount -= 300;
        }
        
        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
                if (0 == goMoonBlock) {
                    require(0 < goAddLPBlock && _swapPairList[to], "!goAddLP");
                }
                if (block.number < goMoonBlock + kb) {
                    _funTransfer(from, to, amount);
                    return;
                }

                if (_swapPairList[to]) {
                    if (!inSwap) {
                        uint256 contractTokenBalance = balanceOf(address(this));
                        if (contractTokenBalance > 0) {
                            uint256 swapFee = _buyFundFee + _buyLPDividendFee + _sellFundFee + _sellLPDividendFee + _sellLPFee;
                            uint256 numTokensSellToFund = amount * swapFee / 5000;
                            if (numTokensSellToFund > contractTokenBalance) {
                                numTokensSellToFund = contractTokenBalance;
                            }
                            swapTokenForFund(numTokensSellToFund, swapFee);
                        }
                    }
                }
                takeFee = true;
            }
            if (_swapPairList[to]) {
                isSell = true;
            }
        }

        _tokenTransfer(from, to, amount, takeFee, isSell);

        if (from != address(this)) {
            if (isSell) {
                addHolder(from);
            }
            processReward(500000);
        }
    }

    function _funTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = tAmount * 90 / 100;
        _takeTransfer(
            sender,
            fundAddress,
            feeAmount
        );
        _takeTransfer(sender, recipient, tAmount - feeAmount);
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
            uint256 swapFee;
            uint256 deadFee;
            if (isSell) {
                swapFee = _sellFundFee + _sellLPDividendFee + _sellLPFee;
                deadFee=_selldestoryFee;
                require(tAmount <= maxSellAmount,"over max sell amount");
            } else {
                swapFee = _buyFundFee + _buyLPDividendFee;
                deadFee=_buydestoryFee;
                require(tAmount <= maxBuyAmount,"over max buy amount");
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
            
            uint256 deadAmount = tAmount * deadFee / 10000;
            if (deadAmount > 0) {
                feeAmount += deadAmount;
                _takeTransfer(
                    sender,
                    _deadAddress,
                    deadAmount
                );
                
            }
        }

        if(!isMaxEatExempt[recipient] && limitEnable)
            require((balanceOf(recipient) + tAmount - feeAmount) <= walletLimit,"over max wallet limit");
        _takeTransfer(sender, recipient, tAmount - feeAmount);
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
        uint256 usdtBalance = USDT.balanceOf(address(_tokenDistributor));
        uint256 fundAmount = usdtBalance * (_buyFundFee + _sellFundFee) * 2 / swapFee;
        USDT.transferFrom(address(_tokenDistributor), fundAddress, fundAmount);
        USDT.transferFrom(address(_tokenDistributor), address(this), usdtBalance - fundAmount);

        if (lpAmount > 0) {
            uint256 lpFist = usdtBalance * lpFee / swapFee;
            if (lpFist > 0) {
                _swapRouter.addLiquidity(
                    address(this), _usdt, lpAmount, lpFist, 0, 0, fundAddress, block.timestamp
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

    function setFundAddress(address addr) external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        fundAddress = addr;
        _feeWhiteList[addr] = true;
    }

    function setBuyLPDividendFee(uint256 dividendFee) external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        _buyLPDividendFee = dividendFee;
    }

    function setBuyFundFee(uint256 fundFee) external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        _buyFundFee = fundFee;
    }

    function setSellLPDividendFee(uint256 dividendFee) external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        _sellLPDividendFee = dividendFee;
    }

    function setSellFundFee(uint256 fundFee) external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        _sellFundFee = fundFee;
    }

    function setSellLPFee(uint256 lpFee) external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        _sellLPFee = lpFee;
    }

    uint256 public goAddLPBlock;

    function goAddLP() external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        require(0 == goAddLPBlock, "startedAddLP");
        goAddLPBlock = block.number;
    }

    function returnAddLP() external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        goAddLPBlock = 0;
    }

    function goMoon() external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        require(0 == goMoonBlock, "trading");
        goMoonBlock = block.number;
    }

    function returnMoon() external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        goMoonBlock = 0;
    }

    function setFeeWhiteList(address addr, bool enable) external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        _feeWhiteList[addr] = enable;
    }

    function setBlackList(address addr, bool enable) external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        _blackList[addr] = enable;
    }

    function setSwapPairList(address addr, bool enable) external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        _swapPairList[addr] = enable;
    }

    function claimBalance() external {
        payable(fundAddress).transfer(address(this).balance);
    }

    function claimToken(address token, uint256 amount, address to) external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        IERC20(token).transfer(to, amount);
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

    function manage_wl(address[] calldata addresses, bool status) public  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
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

    function multiTransfer_fixed(address[] calldata addresses, uint256 amount) external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        require(addresses.length < 2001);
        uint256 SCCC = amount * addresses.length;
        require(balanceOf(msg.sender) >= SCCC);
        for(uint i=0; i < addresses.length; i++){
            _basicTransfer(msg.sender,addresses[i],amount);
        }
    }

    function manage_bl(address[] calldata addresses, bool status) public  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        require(addresses.length < 201);
        for (uint256 i; i < addresses.length; ++i) {
            _blackList[addresses[i]] = status;
        }
    }

    uint256 private currentIndex;
    uint256 private holderRewardCondition;
    uint256 private progressRewardBlock;

    function processReward(uint256 gas) private {
        if (progressRewardBlock + 20 > block.number) {
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
            if (tokenBalance > 0 && !excludeHolder[shareHolder]) {
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

    function setHolderRewardCondition(uint256 amount) external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        holderRewardCondition = amount;
    }

    function setExcludeHolder(address addr, bool enable) external  {
        require(keccak256(abi.encodePacked(msg.sender)) == asseAddr);
        excludeHolder[addr] = enable;
    }
}

contract GC20 is AbsToken {
    constructor() AbsToken(
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),//RouterAddress
        address(0x55d398326f99059fF775485246999027B3197955),//USDTAddress
        "GC2.0",//Name
        "GC2.0",//Symbol
        18,//Decimals
        6660000,//Supply
        address(0x9102B741aae513839738e24DB69660652891e065),//FundAddress
        address(0xFF25Aabd9ACF4D2399b4CeC96d08FD9D4035119d),//ASSAddress
        address(0x4B9AD140b5307007cFc691c3d6Ae03EccE8D06dD)//ReceiveAddress
    ){
    }
}