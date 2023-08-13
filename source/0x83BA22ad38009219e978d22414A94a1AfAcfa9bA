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
interface ISwapPair {
    function getReserves()
        external
        view
        returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function token0() external view returns (address);

    function balanceOf(address account) external view returns (uint256);

    function totalSupply() external view returns (uint256);
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

    address public fundAddress = address(0x08B71936Ac4B01DDaEd920534213c32Aa5aCfe07);
    string private _name = "Pi";
    string private _symbol = "Pi";
    uint8 private _decimals = 18;

    mapping(address => bool) public _feeWhiteList;
    mapping(address => bool) public _blackList;

    uint256 private _tTotal = 36666 * 10 ** _decimals;
    uint256 public maxWalletAmount = 36666 * 10 ** _decimals;

    ISwapRouter public _swapRouter;
    address public _usdt = address(0x55d398326f99059fF775485246999027B3197955);
    address public _routeAddress= address(0x10ED43C718714eb63d5aA57B78B54704E256024E);
    mapping(address => bool) public _swapPairList;
    
    mapping(address => address) public _inviteList;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public _tokenDistributor;

    uint256 public _buyFundFee = 70;
    uint256 public _buyLPDividendFee = 50;
    uint256 public _buyLPFee = 20;
    uint256 public _buyInviteFee=50;
    uint256 public _buyInviteFee1=10;
    uint256 public _buyBurnFee=100;

    uint256 public _sellFundFee = 70;
    uint256 public _sellLPDividendFee = 50;
    uint256 public _sellLPFee = 20;
    uint256 public _sellInviteFee=50;
    uint256 public _sellInviteFee1=10;
    uint256 public _sellBurnFee=100;

    address public _mainPair;
    address private deadAddress=0x000000000000000000000000000000000000dEaD;

    uint256 public lpGasLimit=300000;

    
    uint256 public startTradeTime;
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (){
        ISwapRouter swapRouter = ISwapRouter(_routeAddress);
        IERC20(_usdt).approve(address(swapRouter), MAX);
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address swapPair = swapFactory.createPair(address(this), _usdt);
        _mainPair = swapPair;
        _swapPairList[swapPair] = true;

        _balances[msg.sender] = _tTotal;
        emit Transfer(address(0), msg.sender, _tTotal);
        _feeWhiteList[fundAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;
        _tokenDistributor = new TokenDistributor(_usdt);
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

    function _isAddLiquidity() internal view returns (bool isAdd) {
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0, uint256 r1, ) = mainPair.getReserves();

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

    function _isRemoveLiquidity() internal view returns (bool isRemove) {
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0, uint256 r1, ) = mainPair.getReserves();

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
        bool isAddLP;
        bool isRemoveLP;
        if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
            if (_swapPairList[from] || _swapPairList[to]) {
                if (_swapPairList[to]) {
                    isAddLP = _isAddLiquidity();
                }
                if (_swapPairList[from]) {
                    isRemoveLP = _isRemoveLiquidity();
                }
                if (0 == startTradeTime || block.timestamp< startTradeTime) {
                    require(_swapPairList[to] && isAddLP, "!startAddLP");
                }
                //sell
                if (_swapPairList[to]) {
                    if (!inSwap) {
                        uint256 contractTokenBalance = balanceOf(address(this));
                        if (contractTokenBalance > 0) {
                            uint256 swapFee = _buyFundFee +  _buyLPFee + _buyLPDividendFee  + _sellFundFee + _sellLPFee + _sellLPDividendFee;
                            uint256 numTokensSellToFund = amount * swapFee / 5000;
                            if (numTokensSellToFund > contractTokenBalance) {
                                numTokensSellToFund = contractTokenBalance;
                            }
                            swapTokenForFund(numTokensSellToFund, swapFee);
                        }
                    }
                }
                takeFee = true;
                if(isAddLP||isRemoveLP){
                    takeFee=false;
                }    
            }else{
                // transfer
                if(amount>=0.00001 ether&&from!=address(this)){
                    if(_inviteList[to]==address(0)){
                        _inviteList[to]=from;
                    }
                }
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
            if(takeFee)
            {
               processReward(lpGasLimit);
            }    
        }
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
            uint256 swapInviteFee;
            address swapInviteAddress=fundAddress;
            uint256 swapInviteFee1;
            address swapInviteAddress1=fundAddress;
            uint256 burnFee;
            if (isSell) {
                swapFee = _sellFundFee + _sellLPFee + _sellLPDividendFee;
                swapInviteFee= _sellInviteFee;
                swapInviteFee1=_sellInviteFee1;
                burnFee=_sellBurnFee;
                if(_inviteList[sender]!=address(0)){
                    swapInviteAddress=_inviteList[sender];
                }
                if(_inviteList[swapInviteAddress]!=address(0)){
                    swapInviteAddress1=_inviteList[swapInviteAddress];
                }
            } else {
                require(balanceOf(recipient)+tAmount <= maxWalletAmount);
                swapFee = _buyFundFee + _buyLPFee + _sellLPDividendFee;
                swapInviteFee= _buyInviteFee;
                swapInviteFee1=_buyInviteFee1;
                burnFee=_buyBurnFee;
                if(_inviteList[recipient]!=address(0)){
                    swapInviteAddress=_inviteList[recipient];
                }
                if(_inviteList[swapInviteAddress]!=address(0)){
                    swapInviteAddress1=_inviteList[swapInviteAddress];
                }
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
            if ((tAmount * swapInviteFee / 10000) > 0) {
                feeAmount += (tAmount * swapInviteFee / 10000);
                _takeTransfer(
                    sender,
                    swapInviteAddress,
                    (tAmount * swapInviteFee / 10000)
                );
            }
            if ((tAmount * swapInviteFee1 / 10000) > 0) {
                feeAmount += (tAmount * swapInviteFee1 / 10000);
                _takeTransfer(
                    sender,
                    swapInviteAddress1,
                    (tAmount * swapInviteFee1 / 10000)
                );
            }
            if ((tAmount * burnFee / 10000) > 0) {
                feeAmount += (tAmount * burnFee / 10000);
                _takeTransfer(
                    sender,
                    deadAddress,
                    (tAmount * burnFee / 10000)
                );
            }
        }
        _takeTransfer(sender, recipient, tAmount - feeAmount);

    }

    function swapTokenForFund(uint256 tokenAmount, uint256 swapFee) private lockTheSwap {
        swapFee += swapFee;
        uint256 lpFee = _buyLPFee+_sellLPFee;
        uint256 lpAmount = tokenAmount * lpFee / swapFee;
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _usdt;
        address swapTokenAddress=address(_tokenDistributor);
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(tokenAmount - lpAmount, 0, path,swapTokenAddress,block.timestamp);
        swapFee -= lpFee;
        IERC20 USDT = IERC20(_usdt);
        uint256 usdtBalance = USDT.balanceOf(address(_tokenDistributor));
        if(usdtBalance>0)
        {
            uint256 fundAmount = usdtBalance * (_buyFundFee + _sellFundFee) * 2 / swapFee;
            USDT.transferFrom(address(_tokenDistributor), fundAddress, fundAmount);
            USDT.transferFrom(address(_tokenDistributor), address(this), usdtBalance - fundAmount);
            if (lpAmount > 0) {
                uint256 lpUSDT = usdtBalance * lpFee / swapFee;
                if (lpUSDT > 0) {
                    _swapRouter.addLiquidity(
                        address(this), _usdt, lpAmount, lpUSDT, 0, 0, fundAddress, block.timestamp
                    );
                }
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
    function setMaxWalletAmount(uint256 value) external onlyOwner {
        maxWalletAmount = value * 10 ** _decimals;
    }
    function setGasLimit(uint256 value) external onlyOwner {
        lpGasLimit=value;
    }

    function excludeMultiFromFee(address[] calldata accounts,bool excludeFee) public onlyOwner {
        for(uint256 i = 0; i < accounts.length; i++) {
            _feeWhiteList[accounts[i]] = excludeFee;
        }
    }
    function _multiSetSniper(address[] calldata accounts,bool isSniper) external onlyOwner {
        for(uint256 i = 0; i < accounts.length; i++) {
            _blackList[accounts[i]] = isSniper;
        }
    }
    
    
    function startTrade(uint256 orderedTime) external onlyOwner() {
        require(0 == startTradeTime, "trading");
        startTradeTime = orderedTime;
    }

    function closeTrade() external onlyOwner() {
        startTradeTime = 0;
    }

    function claimBalance(address to) external onlyOwner {
        payable(to).transfer(address(this).balance);
    }

    function claimToken(address token, uint256 amount, address to) external onlyOwner {
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

    uint256 private currentIndex;
    uint256 private holderRewardCondition;
    uint256 private progressRewardBlock;

    function processReward(uint256 gas) private {
        if (progressRewardBlock + 100 > block.number) {
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
                progressRewardBlock = block.number;
            }
            shareHolder = holders[currentIndex];
            tokenBalance = holdToken.balanceOf(shareHolder);
            if (tokenBalance > 0 && !excludeHolder[shareHolder] && balanceOf(shareHolder)>=0.1 ether) {
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
    }
    function setHolderRewardCondition(uint256 amount) external onlyOwner {
        holderRewardCondition = amount;
    }
    function setExcludeHolder(address addr, bool enable) external onlyOwner {
        excludeHolder[addr] = enable;
    }

}

contract Token is AbsToken {
    constructor() AbsToken(){}
}