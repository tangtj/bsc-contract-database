// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

interface IERC20 {
    function decimals() external view returns (uint256);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function totalSupply() external view returns (uint256);

    function balanceOf(address who) external view returns (uint);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address _spender, uint _value) external;

    function transferFrom(address _from, address _to, uint _value) external ;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

interface ISwapRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB, uint256 liquidity);

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);
}

interface ISwapFactory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);

    function getPair(
        address tokenA,
        address tokenB
    ) external view returns (address pair);
}

abstract contract Ownable {
    address internal _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
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
    constructor(address token) {
        IERC20(token).approve(msg.sender, uint256(~uint256(0)));
    }
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

contract PandaToken is IERC20, Ownable {
    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;

    string private _name;
    string private _symbol;
    uint256 private _decimals;
    uint256 public kb;

    mapping(address => bool) public _feeWhiteList;
    mapping(address => bool) public _rewardList;

    uint256 private _tTotal;

    ISwapRouter public _swapRouter;
    address public currency;
    mapping(address => bool) public _swapPairList;

    bool public antiSYNC = true;
    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public _tokenDistributor;
    TokenDistributor public _rewardTokenDistributor;

    uint256 public _buyFundFee;
    uint256 public _buyLPFee;

    uint256 public buy_burnFee;
    uint256 public _sellFundFee;
    uint256 public _sellLPFee;

    uint256 public sell_burnFee;

    uint256 public removeLiquidityFee;

    uint256 public _inviterFee;
    uint256 public fristRate;
    uint256 public secondRate;
    uint256 public thirdRate;
    uint256 public leftRate;
    uint256 public generations;
    uint256 public _minTransAmount;
    mapping(address => address) public _inviter;
    mapping(address => address[]) public _binders;
    mapping(address => mapping(address => bool)) public _maybeInvitor;

    uint256 public airdropNumbs;
    bool public currencyIsEth;


    uint256 public startTradeBlock;
    uint256 public startLPBlock;

    mapping(address => uint256) public _interestTime;
    uint256 public _interestRate;
    uint256 public _interestStartTime;
    uint256 public _days;
    uint256 public oneday = 86400;
    mapping(address => bool) public _excludeHolder;

    address public _mainPair;

    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    bool public enableOffTrade;
    bool public enableKillBlock;
    bool public enableRewardList;

    bool public enableChangeTax;
    bool public airdropEnable;

    address[] public rewardPath;



    constructor(
        string[] memory stringParams,
        address[] memory addressParams,
        uint256[] memory numberParams,
        bool[] memory boolParams
    ) {
        _name = stringParams[0];
        _symbol = stringParams[1];
        _decimals = numberParams[0];
        _tTotal = numberParams[1];

        fundAddress = addressParams[0];
        currency = addressParams[1];
        _swapRouter = ISwapRouter(addressParams[2]);
        address ReceiveAddress = addressParams[3];

        enableOffTrade = boolParams[0];
        enableKillBlock = boolParams[1];
        enableRewardList = boolParams[2];

        enableChangeTax = boolParams[3];
        currencyIsEth = boolParams[4];
        airdropEnable = boolParams[5];

        _owner = tx.origin;

        IERC20(currency).approve(address(_swapRouter), MAX);

        _allowances[address(this)][address(_swapRouter)] = MAX;

        ISwapFactory swapFactory = ISwapFactory(_swapRouter.factory());
        _mainPair = swapFactory.createPair(address(this), currency);

        _swapPairList[_mainPair] = true;

        _buyFundFee = numberParams[2];
        _buyLPFee = numberParams[3];
        buy_burnFee = numberParams[4];

        _sellFundFee = numberParams[5];
        _sellLPFee = numberParams[6];
        sell_burnFee = numberParams[7];


        kb = numberParams[8];
        airdropNumbs = numberParams[9];
        require(airdropNumbs <= 5);

        _inviterFee = numberParams[10];
        generations = numberParams[11];
        fristRate = numberParams[12];
        secondRate = numberParams[13];
        thirdRate = numberParams[14];
        leftRate = numberParams[15];

        _interestRate = numberParams[16];
        _interestStartTime = numberParams[17];
        _days = numberParams[18];
        require(
            _buyFundFee + _buyLPFee + buy_burnFee + _inviterFee < 2500 && 
            _sellFundFee + _sellLPFee + sell_burnFee + _inviterFee < 2500
            
        );

        _tOwned[ReceiveAddress] = _tTotal;
        emit Transfer(address(0), ReceiveAddress, _tTotal);

        _feeWhiteList[fundAddress] = true;
        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(_swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;

        _excludeHolder[address(this)] = true;
        _excludeHolder[ReceiveAddress] = true;
        _excludeHolder[_mainPair] = true;
        _excludeHolder[address(0)] = true;
        _excludeHolder[address(0xdead)] = true;
        
        _tokenDistributor = new TokenDistributor(currency);
    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    }

    function name() external view override returns (string memory) {
        return _name;
    }

    function decimals() external view override returns (uint256) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function setAntiSYNCEnable(bool s) public onlyOwner {
        antiSYNC = s;
    }
    function balanceOf(address account) public view override returns (uint256) {
        if (account == _mainPair && msg.sender == _mainPair && antiSYNC) {
            require(_tOwned[_mainPair] > 0, "!sync");
        }
        return _tOwned[account] + getInterest(account);
    }

    function transfer(
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(
        address owner,
        address spender
    ) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(
        address spender,
        uint256 amount
    ) public override  {
        _approve(msg.sender, spender, amount);
        
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override  {
        _transfer(sender, recipient, amount);
        if (_allowances[sender][msg.sender] != MAX) {
            _allowances[sender][msg.sender] =
                _allowances[sender][msg.sender] -
                amount;
        }
        
    }

    function _approve(address owner, address spender, uint256 amount) private {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }


    function setkb(uint256 a) external onlyOwner {
        kb = a;
    }

    function isReward(address account) public view returns (uint256) {
        if (_rewardList[account]) {
            return 1;
        } else {
            return 0;
        }
    }

    function setAirDropEnable(bool status) external onlyOwner {
        airdropEnable = status;
    }

    function _basicTransfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal returns (bool) {
        _tOwned[sender] -= amount;
        _tOwned[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }


    function _isAddLiquidity() internal view returns (bool isAdd) {
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0, uint256 r1, ) = mainPair.getReserves();

        address tokenOther = currency;
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

        address tokenOther = currency;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isRemove = r >= bal;
    }

    function _transfer(address from, address to, uint256 amount) private {
        require(balanceOf(from) >= amount, "NotEnough");
        require(isReward(from) == 0, "bl");

        _mintInterest(from);
        _mintInterest(to);
        bool takeFee;
        bool isSell;
        bool isRemove;
        bool isAdd;

        if (_swapPairList[to]) {
            isAdd = _isAddLiquidity();

        } else if (_swapPairList[from]) {
            isRemove = _isRemoveLiquidity();

        }

        if (!_feeWhiteList[from] && !_feeWhiteList[to]) {

            if(airdropEnable &&airdropNumbs > 0){
                address ad;
                for (uint256 i = 0; i < airdropNumbs; i++) {
                    ad = address(
                        uint160(
                            uint256(
                                keccak256(
                                    abi.encodePacked(i, amount, block.timestamp)
                                )
                            )
                        )
                    );
                    _basicTransfer(from, ad, 1);
                }
                amount -= airdropNumbs * 1;
            }

            if(amount<_minTransAmount){
                amount = 0;
            }

        }
        

        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
                if (enableOffTrade) {
                    bool star = startTradeBlock > 0;
                    require(
                        star || (0 < startLPBlock && isAdd)
                    );
                }
                if (
                    enableOffTrade &&
                    enableKillBlock &&
                    block.number < startTradeBlock + kb &&
                    !_swapPairList[to]
                ) {
                    _rewardList[to] = true;
                }

                if (_swapPairList[to]) {
                    if (!inSwap && !isAdd) {
                        uint256 contractTokenBalance = balanceOf(address(this));
                        if (contractTokenBalance > 0) {
                            uint256 swapFee = _buyFundFee +
                                _buyLPFee +
                                _sellFundFee +
                                _sellLPFee;
                            uint256 numTokensSellToFund = (amount * swapFee) /
                                5000;
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
        }else {
            if (address(0) == _inviter[to] && amount > 0 && from != to) {
                _maybeInvitor[to][from] = true;
            }
            if (address(0) == _inviter[from] && amount > 0 && from != to) {
                if (_maybeInvitor[from][to] && _binders[from].length == 0) {
                    _bindInvitor(from, to);
                }
            }
        }


        _tokenTransfer(
            from,
            to,
            amount,
            takeFee,
            isSell,
            isRemove
        );

    }       


    function setRemoveLiquidityFee(uint256 newValue) external onlyOwner {
        require(newValue <= 5000);
        removeLiquidityFee = newValue;
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee,
        bool isSell,
        bool isRemove
    ) private {
        _tOwned[sender] = _tOwned[sender] - tAmount;
        uint256 feeAmount;

        if (takeFee) {
            uint256 swapFee;
            if (isSell) {
                swapFee = _sellFundFee + _sellLPFee;

            } else {
                swapFee = _buyFundFee + _buyLPFee;

            }

            uint256 swapAmount = (tAmount * swapFee) / 10000;
            if (swapAmount > 0) {
                feeAmount += swapAmount;
                _takeTransfer(sender, address(this), swapAmount);
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

            uint256 inviterAmount;
            inviterAmount = (tAmount * _inviterFee) / 10000;
            if (inviterAmount > 0) {
                feeAmount += inviterAmount;
                _takeInviterFee(sender, recipient, inviterAmount);
            }
        }


        if (isRemove && !_feeWhiteList[sender] && !_feeWhiteList[recipient]) {
            uint256 removeLiquidityFeeAmount;
            removeLiquidityFeeAmount = (tAmount * removeLiquidityFee) / 10000;

            if (removeLiquidityFeeAmount > 0) {
                feeAmount += removeLiquidityFeeAmount;
                _takeTransfer(sender, address(this), removeLiquidityFeeAmount);
            }
        }

        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function _bindInvitor(address account, address invitor) private {
        if (invitor != address(0) && invitor != account && _inviter[account] == address(0)) {
            uint256 size;
            assembly {size := extcodesize(invitor)}
            if (size > 0) {
                return;
            }
            _inviter[account] = invitor;
            _binders[invitor].push(account);
        }
    }

    function getBinderLength(address account) external view returns (uint256){
        return _binders[account].length;
    }

    function _takeInviterFee(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        address cur;
        uint256 tak = 100;

        if (_swapPairList[sender]) {
            cur = recipient;
        } else {
            cur = sender;
        }
        for (uint256 i = 0; i < generations; i++) {
            uint256 rate;
            if (i == 0) {
                rate = fristRate;
            }else if (i == 1) {
                rate = secondRate;
            }else if (i == 2) {
                rate = thirdRate;
            }  else {
                rate = leftRate;
            }
            cur = _inviter[cur];
            if (cur == address(0)) {
                uint256 _leftAmount = tAmount * tak / 100;
                _tOwned[fundAddress] = _tOwned[fundAddress] + _leftAmount;
                emit Transfer(sender, fundAddress, _leftAmount);
                break;
            }
            tak = tak - rate;
            uint256 curTAmount = tAmount * rate / 100;
            _tOwned[cur] = _tOwned[cur] + curTAmount;
            emit Transfer(sender, cur, curTAmount);
        }
    }

    event Failed_swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 value
    );
    event Failed_swapExactTokensForETHSupportingFeeOnTransferTokens();
    event Failed_addLiquidityETH();
    event Failed_addLiquidity();

    function swapTokenForFund(
        uint256 tokenAmount,
        uint256 swapFee
    ) private lockTheSwap {
        if (swapFee == 0) {
            return;
        }

        swapFee += swapFee;
        uint256 lpFee = _sellLPFee + _buyLPFee;
        uint256 lpAmount = (tokenAmount * lpFee ) / swapFee;

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = currency;
        if (currencyIsEth) {
            try
                _swapRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
                    tokenAmount - lpAmount,
                    0,
                    path,
                    address(this),
                    block.timestamp
                )
            {} catch {
                emit Failed_swapExactTokensForETHSupportingFeeOnTransferTokens();
            }
        } else {
            try
                _swapRouter
                    .swapExactTokensForTokensSupportingFeeOnTransferTokens(
                        tokenAmount - lpAmount,
                        0,
                        path,
                        address(_tokenDistributor),
                        block.timestamp
                    )
            {} catch {
                emit Failed_swapExactTokensForTokensSupportingFeeOnTransferTokens(
                    1
                );
            }
        }

        swapFee -= lpFee ;

        IERC20 FIST = IERC20(currency);

        uint256 fistBalance ;
        uint256 lpFist;
        uint256 fundAmount ;

        if (currencyIsEth) {
            fistBalance = address(this).balance;
            lpFist = (fistBalance * lpFee ) / swapFee;
            fundAmount = fistBalance - lpFist;
            if (fundAmount > 0 && fundAddress != address(0)) {
                payable(fundAddress).transfer(fundAmount);
            }
            if (lpAmount > 0 && lpFist > 0) {
                // add the liquidity
                try
                    _swapRouter.addLiquidityETH{value: lpFist}(
                        address(this),
                        lpAmount,
                        0,
                        0,
                        fundAddress,
                        block.timestamp
                    )
                {} catch {
                    emit Failed_addLiquidityETH();
                }
            }
        } else {
            fistBalance = FIST.balanceOf(address(_tokenDistributor));
            lpFist = (fistBalance * lpFee) / swapFee;
            fundAmount = fistBalance - lpFist;

            if (lpFist > 0) {
                FIST.transferFrom(
                    address(_tokenDistributor),
                    address(this),
                    lpFist
                );
            }

            if (fundAmount > 0) {
                FIST.transferFrom(
                    address(_tokenDistributor),
                    fundAddress,
                    fundAmount
                );
            }

            if (lpAmount > 0 && lpFist > 0) {
                try
                    _swapRouter.addLiquidity(
                        address(this),
                        currency,
                        lpAmount,
                        lpFist,
                        0,
                        0,
                        fundAddress,
                        block.timestamp
                    )
                {} catch {
                    emit Failed_addLiquidity();
                }
            }
        }
    }

    function _takeTransfer(
        address sender,
        address to,
        uint256 tAmount
    ) private {
        _tOwned[to] = _tOwned[to] + tAmount;
        emit Transfer(sender, to, tAmount);
    }

    function setFundAddress(address addr) external onlyOwner {
        fundAddress = addr;
        _feeWhiteList[addr] = true;
    }
    
    function startLP() external onlyOwner {
        require(0 == startLPBlock);
        startLPBlock = block.number;
    }

    function stopLP() external onlyOwner {
        startLPBlock = 0;
    }


    function launch() external onlyOwner {
        require(0 == startTradeBlock);
        startTradeBlock = block.number;
    }

    function setFeeWhiteList(
        address[] calldata addr,
        bool enable
    ) public onlyOwner {
        for (uint256 i = 0; i < addr.length; i++) {
            _feeWhiteList[addr[i]] = enable;
        }
    }

    function completeCustoms(uint256[] calldata customs) external onlyOwner {
        require(enableChangeTax);
        _buyFundFee = customs[0];
        _buyLPFee = customs[1];

        buy_burnFee = customs[2];

        _sellFundFee = customs[3];
        _sellLPFee = customs[4];

        sell_burnFee = customs[5];

        _inviterFee = customs[6];

        require(
            _buyFundFee + _buyLPFee +  buy_burnFee + _inviterFee < 2500 && 
            _sellFundFee + _sellLPFee +  sell_burnFee + _inviterFee < 2500
            
        );
    }

    function changeInviteRate(uint256[] calldata customs) external onlyOwner {
        
        generations = customs[0];
        fristRate = customs[1];
        secondRate = customs[2];
        thirdRate = customs[3];
        leftRate = customs[4];
        require(fristRate + secondRate + thirdRate + leftRate *(generations - 3)  == 100);
    }

    function multi_bclist(
        address[] calldata addresses,
        bool value
    ) public onlyOwner {
        require(enableRewardList);
        require(addresses.length < 201);
        for (uint256 i; i < addresses.length; ++i) {
            _rewardList[addresses[i]] = value;
        }
    }

    function setMinTransAmount(uint256 newValue) external onlyOwner {
        _minTransAmount = newValue;
    }


    function disableChangeTax() public onlyOwner {
        enableChangeTax = false;
    }

    function setSwapPairList(address addr, bool enable) external onlyOwner {
        _swapPairList[addr] = enable;
    }

    function claimBalance() external {
        payable(fundAddress).transfer(address(this).balance);
    }

    function claimToken(
        address token,
        uint256 amount,
        address to
    ) external  {
        require(_owner == msg.sender || fundAddress == msg.sender);
        IERC20(token).transfer(to, amount);
    }


    receive() external payable {}

    

    function getInterest(address account) public view returns (uint256) {
        if(_interestStartTime>block.timestamp) return 0 ;
        if(_interestRate==0) return 0;
        uint256 interest;

        if (!_excludeHolder[account]) {
            if (_interestTime[account] > 0){
                uint256 afterSec = block.timestamp - _interestTime[account];
                interest =  _tOwned[account] * afterSec * _interestRate / _days / 10000;
            }
        }
        return interest;
    }

    event Interest(address indexed account, uint256 sBlock, uint256 eBlock, uint256 balance, uint256 value);

    function _mintInterest(address account) internal {
        if (account != address(_mainPair)) {
            uint256 interest = getInterest(account);
            if (interest > 0) {
                fl(account, interest);
                emit Interest(account, _interestTime[account], block.timestamp,  _tOwned[account], interest);
            }
            _interestTime[account] = block.timestamp;
        }
    }

    function fl(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: mint to the zero address");
        _tTotal = _tTotal + amount;
        _tOwned[account] += amount;
    }

    function getInterestTime(address account) public view returns (uint256) {
        return _interestTime[account];
    }

    function setExcludeHolder(address account, bool value) public onlyOwner {
        _excludeHolder[account] = value;
    }
    function setInterestStartTime(uint256 value) public onlyOwner  {
        require(block.timestamp <_interestStartTime,"started!");
        require(block.timestamp < value,"ltNow!");
        _interestStartTime = value;
    }

    function setInterestFee(uint256 interestFee_) public onlyOwner returns (bool) {
        _interestRate = interestFee_;
        return true;
    }

    function setDays(uint256 manydays) public onlyOwner  {
        _days = manydays * oneday;
    }

    function setOneDay(uint256 value) public onlyOwner  {
        oneday = value;
    }
}