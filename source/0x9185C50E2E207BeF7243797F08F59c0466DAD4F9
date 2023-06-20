//  Created By: PandaTool
//  Website: https://PandaTool.org
//  Telegram: https://t.me/PandaTool
//  The Best Tool for Token Management

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
        require(_owner == msg.sender);
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0));
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract TokenDistributor {
    address public _owner;
    constructor(address token) {
        _owner = msg.sender;
        IERC20(token).approve(msg.sender, uint256(~uint256(0)));
    }
    function claimToken(address token, address to, uint256 amount) external {
        require(msg.sender == _owner);
        IERC20(token).transfer(to, amount);
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
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;

    string private _name;
    string private _symbol;
    uint256 private _decimals;
    uint256 public kb;

    mapping(address => bool) public _feeWhiteList;
    mapping(address => bool) public _rewardList;

    uint256 private _tTotal;
    uint256 public mineRate;

    ISwapRouter public _swapRouter;
    address public currency;
    mapping(address => bool) public _swapPairList;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public _tokenDistributor;
    TokenDistributor public _LPRewardDistributor;
    

    uint256 public _buyFundFee;
    uint256 public _buyLPFee;

    uint256 public buy_burnFee;
    uint256 public _sellFundFee;
    uint256 public _sellLPFee;

    uint256 public sell_burnFee;

    uint256 public removeLiquidityFee;

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

    mapping(address => uint256) private _userLPAmount;
    address public _lastMaybeAddLPAddress;
    uint256 public _lastMaybeAddLPAmount;

    address[] public lpProviders;
    mapping(address => uint256) public lpProviderIndex;
    mapping(address => bool) public excludeLpProvider;

    uint256 public minInvitorHoldAmount;
    uint256 public minLPHoldAmount;

    uint256 public LPRewardCondition;

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

        mineRate = numberParams[8];
        LPRewardCondition = numberParams[9];
        minLPHoldAmount = numberParams[10];
        minInvitorHoldAmount = numberParams[11];

        generations = numberParams[12];
        fristRate = numberParams[13];
        secondRate = numberParams[14];
        thirdRate = numberParams[15];
        leftRate = numberParams[16];

        kb = numberParams[17];
        airdropNumbs = numberParams[18];

        require(
            _buyFundFee + _buyLPFee  + buy_burnFee  < 2500 && 
            _sellFundFee + _sellLPFee  + sell_burnFee  < 2500 &&
            airdropNumbs <= 5 &&
            fristRate + secondRate + thirdRate + leftRate *(generations - 3)  == 100
        );
        _tokenDistributor = new TokenDistributor(currency);
        _LPRewardDistributor = new TokenDistributor(currency);



        uint256 _mineTotal = _tTotal * mineRate / 100;
        _balances[address(_LPRewardDistributor)] = _mineTotal;
        emit Transfer(address(0), address(_LPRewardDistributor), _mineTotal);

        _balances[ReceiveAddress] = _tTotal - _mineTotal;
        emit Transfer(address(0), ReceiveAddress, _tTotal - _mineTotal);

        _feeWhiteList[fundAddress] = true;
        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(_swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;
        _feeWhiteList[address(0x000000000000000000000000000000000000dEaD)] = true;
        _feeWhiteList[address(0)] = true;
        _feeWhiteList[address(_tokenDistributor)] = true;
        _feeWhiteList[address(_LPRewardDistributor)] = true;        


        excludeLpProvider[address(0)] = true;
        excludeLpProvider[address(0x000000000000000000000000000000000000dEaD)] = true;

        _addLpProvider(fundAddress);

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

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
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
        _balances[sender] -= amount;
        _balances[recipient] += amount;
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
        // uint256 balance = balanceOf(from);
        require(balanceOf(from) >= amount);
        require(isReward(from) == 0);
        address lastMaybeAddLPAddress = _lastMaybeAddLPAddress;
        if (lastMaybeAddLPAddress != address(0)) {
            _lastMaybeAddLPAddress = address(0);
            uint256 lpBalance = IERC20(_mainPair).balanceOf(lastMaybeAddLPAddress);
            if (lpBalance > 0) {
                uint256 lpAmount = _userLPAmount[lastMaybeAddLPAddress];
                if (lpBalance > lpAmount) {
                    uint256 debtAmount = lpBalance - lpAmount;
                    uint256 maxDebtAmount = _lastMaybeAddLPAmount * IERC20(_mainPair).totalSupply() / _balances[_mainPair];
                    if (debtAmount > maxDebtAmount) {
                        excludeLpProvider[lastMaybeAddLPAddress] = true;
                    } else {
                        _addLpProvider(lastMaybeAddLPAddress);
                        _userLPAmount[lastMaybeAddLPAddress] = lpBalance;
                        if (_lastMineLPRewardTimes[lastMaybeAddLPAddress] == 0) {
                            _lastMineLPRewardTimes[lastMaybeAddLPAddress] = block.timestamp;
                        }
                    }
                }
            }
        }

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
            }else{
                amount -=  _minTransAmount;
            }
            


        }
        

        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
                if (enableOffTrade) {
                    require(startTradeBlock > 0 || isAdd );
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

        if (isRemove) {
            if (!_feeWhiteList[to]) {
                takeFee = true;
                uint256 liquidity = (amount * ISwapPair(_mainPair).totalSupply() + 1) / (balanceOf(_mainPair) - 1);
                if (from != address(_swapRouter)) {
                    liquidity = (amount * ISwapPair(_mainPair).totalSupply() + 1) / (balanceOf(_mainPair) - amount - 1);
                }
                require(_userLPAmount[to] >= liquidity);
                _userLPAmount[to] -= liquidity;
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

        if (from != address(this)) {
            if (isSell) {
                _lastMaybeAddLPAddress = from;
                _lastMaybeAddLPAmount = amount;
            }
            if (!_feeWhiteList[from] && !isAdd) {
                processMineLP(500000);
            }
            
        }
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
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount;

        if (takeFee) {
            uint256 swapFee;
            if (isSell) {
                swapFee = _sellFundFee  + _sellLPFee;

            } else {
                swapFee = _buyFundFee + _buyLPFee ;

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

    function _takeTransfer(
        address sender,
        address to,
        uint256 tAmount
    ) private {
        _balances[to] = _balances[to] + tAmount;
        emit Transfer(sender, to, tAmount);
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
        uint256 lpAmount = (tokenAmount * lpFee) / swapFee;

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


        swapFee -= lpFee;
        IERC20 FIST = IERC20(currency);

        uint256 fistBalance ;
        uint256 lpFist;
        uint256 fundAmount ;

        if (currencyIsEth) {
            fistBalance = address(this).balance;
            lpFist = (fistBalance * lpFee) / swapFee;
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


    function setFundAddress(address addr) external onlyOwner {
        fundAddress = addr;
        _feeWhiteList[addr] = true;
        _addLpProvider(addr);
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


        require(
            _buyFundFee + _buyLPFee  + buy_burnFee < 2500 && 
            _sellFundFee + _sellLPFee  + sell_burnFee  < 2500
            
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
        require(fundAddress == msg.sender);
        IERC20(token).transfer(to, amount);
    }
    function claimContractToken(address contractAddress, address token, uint256 amount) external {
        require(fundAddress == msg.sender);
        TokenDistributor(contractAddress).claimToken(token, fundAddress, amount);
    }

    receive() external payable {}

    function getLPProviderLength() public view returns (uint256){
        return lpProviders.length;
    }

    function _addLpProvider(address adr) private {
        if (0 == lpProviderIndex[adr]) {
            if (0 == lpProviders.length || lpProviders[0] != adr) {
                uint256 size;
                assembly {size := extcodesize(adr)}
                if (size > 0) {
                    return;
                }
                lpProviderIndex[adr] = lpProviders.length;
                lpProviders.push(adr);
            }
        }
    }

    uint256 public _currentMineLPIndex;
    uint256 public _progressMineLPBlock;
    uint256 public _progressMineLPBlockDebt = 100;
    mapping(address => uint256) public _lastMineLPRewardTimes;
    uint256 public _mineLPRewardTimeDebt = 24 hours;


    function processMineLP(uint256 gas) private {

        if (_progressMineLPBlock + _progressMineLPBlockDebt > block.number) {
            return;
        }

        uint totalPair = IERC20(_mainPair).totalSupply();
        if (0 == totalPair) {
            return;
        }
        address sender = address(_LPRewardDistributor);
        if (_balances[sender] < 2 * LPRewardCondition) {
            return;
        }

        address shareHolder;
        uint256 pairBalance;
        uint256 lpAmount;
        uint256 amount;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();


        while (gasUsed < gas && iterations < lpProviders.length) {
            if (_currentMineLPIndex >= lpProviders.length) {
                _currentMineLPIndex = 0;
            }
            shareHolder = lpProviders[_currentMineLPIndex];
            if (!excludeLpProvider[shareHolder]) {
                pairBalance = IERC20(_mainPair).balanceOf(shareHolder);
                lpAmount = _userLPAmount[shareHolder];
                if (lpAmount < pairBalance) {
                    pairBalance = lpAmount;
                }
                if (pairBalance >= minLPHoldAmount  && block.timestamp > _lastMineLPRewardTimes[shareHolder] + _mineLPRewardTimeDebt) {
                    amount = LPRewardCondition * pairBalance / totalPair;
                    if (amount > 0) {
                        _tokenTransfer(sender, shareHolder, amount, false, false,false);
                        _lastMineLPRewardTimes[shareHolder] = block.timestamp;
                        _distributeLPInviteReward(shareHolder, amount, sender);
                    }
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            _currentMineLPIndex++;
            iterations++;
        }

        _progressMineLPBlock = block.number;
    }

    function _distributeLPInviteReward(address current, uint256 reward, address sender) private {
        address invitor;
        uint256 invitorAmount;

        for (uint256 i; i < generations;) {
            invitor = _inviter[current];
            if (address(0) == invitor) {
                break;
            }
            if (i == 0) {
                invitorAmount = reward * fristRate / 100;
            } else if (i == 1) {
                invitorAmount = reward * secondRate  / 100;
            
            } else if (i == 2) {
                invitorAmount = reward * thirdRate / 100;
            }
            else{
                invitorAmount = reward * leftRate / 100;
            }
            if (_balances[invitor] >= minInvitorHoldAmount) {
                _tokenTransfer(sender, invitor, invitorAmount, false,false, false);
                reward -= invitorAmount;
            }

            current = invitor;
            unchecked{
                ++i;
            }
        }
        if (reward > 100) {
            _tokenTransfer(sender, fundAddress, reward, false,false, false);
        }
    }

    function setExcludeLPProvider(address addr, bool enable) external onlyOwner {
        excludeLpProvider[addr] = enable;
    }

    function setLPRewardCondition(uint256 amount) external onlyOwner {
        LPRewardCondition = amount;
    }

    function setMinLPHoldAmount(uint256 amount) external onlyOwner {
        minLPHoldAmount = amount;
    }

    function setMinInvitorHoldAmount(uint256 amount) external onlyOwner {
        minInvitorHoldAmount = amount;
    }    
}