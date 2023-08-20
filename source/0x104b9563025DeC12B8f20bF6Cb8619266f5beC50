/**
 *  Created By: MsgSender.io & HK MSG Limited
 *  Website: https://msgsender.io
 *  Telegram: https://t.me/MsgSender_AP
 *  msgsender.io, An Easy Acess to Dex
 **/

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

interface IERC20 {
    function decimals() external view returns (uint256);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

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

contract FantasyLand is IERC20, Ownable {

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) public _feeWhiteList;
    mapping(address => bool) public _swapPairList;
    mapping(address => uint256) private holderIndex;
    mapping(address => bool) private excludeHolder;
    mapping(address => uint256) private partnerIndex;
    mapping(address => bool) private excludePartner;
    
    address public _mainPair;
    address public fundAddress;
    address public currency;
    address public initialAddress;
    address[] public rewardPath;
    address[] private holders;
    address[] private partners;

    string private _name;
    string private _symbol;

    uint256 private _decimals;
    uint256 private _tTotal;
    uint256 private constant MAX = ~uint256(0);
    uint256 public startTradeBlock = 50000000;
    uint256 public startLPBlock = 50000000;
    uint256 public kb = 3;
    uint256 public _buyPartnerFee;
    uint256 public _buyRewardFee;
    uint256 public _sellPartnerFee;
    uint256 public _sellBurnFee;
    uint256 public rewardHat;
    uint256 public totalPartnerNumber;
    uint256 private currentIndex;
    uint256 private currentPartnerIndex;
    uint256 public holderRewardCondition;
    uint256 public partnerRewardCondition;
    uint256 public SwapRewardCondition;
    uint256 private progressRewardBlock;
    uint256 private progressPartnerBlock;

    bool private inSwap;
    bool public enableKillBatchBots;
    bool public enableKillBlock;
    bool currencyIsEth;

    ISwapRouter public _swapRouter;
    TokenDistributor public _partnerRewardDistributor;
    TokenDistributor public _rewardTokenDistributor;

    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor() {

        _name = "F123456789";
        _symbol = "F123";
        _decimals = 18;
        _tTotal = 10**14 * 10**18;

        fundAddress = address(0x675D6290465C1d35F004d2d3b3bA28eB5A54E67F);
        currency = address(0x55d398326f99059fF775485246999027B3197955);
        ISwapRouter swapRouter = ISwapRouter(address(0x10ED43C718714eb63d5aA57B78B54704E256024E));
        initialAddress = address(0xA0bF0F7298F6FfFBC06628E3d9eF5f19C1D3AF77);

        enableKillBlock = true;
        rewardPath = [address(this), currency];
        IERC20(currency).approve(address(swapRouter), MAX);
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;
        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address swapPair = swapFactory.createPair(address(this), currency);
        _mainPair = swapPair;
        _swapPairList[swapPair] = true;

        _buyPartnerFee = 100;
        _buyRewardFee = 100;
        _sellPartnerFee = 100;
        _sellBurnFee = 100;

        _balances[initialAddress] = _tTotal;
        emit Transfer(address(0), initialAddress, _tTotal);

        _feeWhiteList[fundAddress] = true;
        _feeWhiteList[initialAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;
        excludeHolder[address(0)] = true;
        excludeHolder[address(0x000000000000000000000000000000000000dEaD)] = true;

        totalPartnerNumber = 0;
        holderRewardCondition = 10 ** 18;
        partnerRewardCondition = 10 * 10 ** 18;
        rewardHat = 10 * 10 ** 18;
        SwapRewardCondition = 10 * 10 ** 18;

        _partnerRewardDistributor = new TokenDistributor(currency);
        _rewardTokenDistributor = new TokenDistributor(currency);
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
    ) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        if (_allowances[sender][msg.sender] != MAX) {
            _allowances[sender][msg.sender] =
                _allowances[sender][msg.sender] -
                amount;
        }
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function setkb(uint256 a) public onlyOwner {
        kb = a;
    }

    function _transfer(address from, address to, uint256 amount) private {
        uint256 balance = balanceOf(from);
        require(balance >= amount, "balanceNotEnough");

        bool takeFee;
        bool isSell;

        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
                require(block.number >= startTradeBlock,"trade not started");
                if (enableKillBlock && block.number < startTradeBlock + kb) {
                    _interTransfer(from, to, amount, 70, fundAddress);
                    return;
                }
                takeFee = true;
            }
            if (_swapPairList[to]) {
                isSell = true;
            }
        } else {
            if (
                block.number < startTradeBlock &&
                !_feeWhiteList[from] &&
                !_feeWhiteList[from]
            ) {
                _interTransfer(from, to, amount, 5, address(0xdead));
                return;
            }
        }

        if (!inSwap && block.number >= startTradeBlock) {
            uint256 contractTokenBalance = balanceOf(address(this));
            if (contractTokenBalance > SwapRewardCondition) {
                uint256 swapFee = 300;
                uint256 numTokensSellToFund = contractTokenBalance;
                swapTokenForFund(numTokensSellToFund, swapFee);
            }
        }

        _launchTransfer(from, to, amount, takeFee, isSell);

        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
                require(block.number >= startTradeBlock, "trade not started");
                if (from != address(this)) {
                    if (isSell) {
                        addHolder(from);
                    }
                    processReward(250000);
                    processPartnerReward(250000);
                }
            }
        }
    }

    function _interTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        uint256 feeRate,
        address feeAddress
    ) private {
        require(feeRate <= 100, "feeRate too high");
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = (tAmount * feeRate) / 100;
        _baseTransfer(sender, feeAddress, feeAmount);
        _baseTransfer(sender, recipient, tAmount - feeAmount);
    }

    function _launchTransfer(
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
            if (isSell) {
                swapFee = _sellPartnerFee;
                uint256 burnAmount = (tAmount * _sellBurnFee) / 10000;
                _baseTransfer(sender, address(0xdead), burnAmount);
            } else {
                swapFee = _buyPartnerFee + _buyRewardFee;
            }

            uint256 swapAmount = (tAmount * swapFee) / 10000;
            if (swapAmount > 0) {
                feeAmount += swapAmount;
                _baseTransfer(sender, address(this), swapAmount);
            }
        }
        _baseTransfer(sender, recipient, tAmount - feeAmount);
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

        uint256 holderAmount = (tokenAmount * _buyRewardFee) / swapFee;
        try
            _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                holderAmount,
                0,
                rewardPath,
                address(_rewardTokenDistributor),
                block.timestamp
            )
        {} catch {
            emit Failed_swapExactTokensForTokensSupportingFeeOnTransferTokens(
                0
            );
        }

        uint256 partnerFee = _sellPartnerFee + _buyPartnerFee;
        uint256 partnerAmount = (tokenAmount * partnerFee) / swapFee;
        try
            _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                partnerAmount,
                0,
                rewardPath,
                address(_partnerRewardDistributor),
                block.timestamp
            )
        {} catch {
            emit Failed_swapExactTokensForTokensSupportingFeeOnTransferTokens(
                0
            );
        }
    }

    function _baseTransfer(
        address sender,
        address to,
        uint256 tAmount
    ) private {
        _balances[to] = _balances[to] + tAmount;
        emit Transfer(sender, to, tAmount);
    }

    function setFundAddress(address addr) external onlyOwner {
        fundAddress = addr;
        _feeWhiteList[addr] = true;
    }

    function startLP() external onlyOwner {
        startLPBlock = block.number;
    }

    function launch() external onlyOwner {
        startTradeBlock = block.number;
    }

    function setWhiteList(
        address[] calldata addr,
        bool enable
    ) public onlyOwner {
        for (uint256 i = 0; i < addr.length; i++) {
            _feeWhiteList[addr[i]] = enable;
        }
    }

    function setCurrency(address _currency) public onlyOwner {
        currency = _currency;
        if (_currency == _swapRouter.WETH()) {
            currencyIsEth = true;
        } else {
            currencyIsEth = false;
        }
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
    ) external onlyFunder {
        IERC20(token).transfer(to, amount);
    }

    modifier onlyFunder() {
        require(_owner == msg.sender || fundAddress == msg.sender, "!Funder");
        _;
    }

    receive() external payable {}

    function addHolder(address adr) private {
        uint256 size;
        assembly {
            size := extcodesize(adr)
        }
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

    function addPartner(address adr) public onlyOwner {
        if (0 == partnerIndex[adr]) {
            if (0 == partners.length || partners[0] != adr) {
                partnerIndex[adr] = partners.length;
                partners.push(adr);
            }
        }
        totalPartnerNumber = totalPartnerNumber + 1;
    }

    function excluedPartner(address adr) public onlyOwner {
        excludePartner[adr] = true;
    }

    function processReward(uint256 gas) private {
        if (progressRewardBlock + 2400 > block.number) {
            return;
        }

        IERC20 REWARD_COIN = IERC20(currency);

        uint256 balance = REWARD_COIN.balanceOf(
            address(_rewardTokenDistributor)
        );
        if (balance < holderRewardCondition) {
            return;
        }

        if (balance > rewardHat) {
            balance = rewardHat;
        }

        IERC20 holdToken = IERC20(_mainPair);
        uint256 holdTokenTotal = holdToken.totalSupply();

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
                amount = (balance * tokenBalance) / holdTokenTotal;
                if (amount > 0) {
                    REWARD_COIN.transferFrom(
                        address(_rewardTokenDistributor),
                        shareHolder,
                        amount
                    );
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }

        progressRewardBlock = block.number;
    }

    function processPartnerReward(uint256 gas) private {


        if (progressPartnerBlock + 2400 > block.number) {
            return;
        }
        IERC20 REWARD_COIN = IERC20(currency);
        uint256 partnerTotalBalance = REWARD_COIN.balanceOf(address(_partnerRewardDistributor));
        if (partnerTotalBalance < partnerRewardCondition) {
            return;
        }


        address partnerAddress;
        uint256 partnerCount = partners.length;
        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();
        uint256 partnerRewardAmount = 10000000000;


        while (gasUsed < gas && iterations < partnerCount) {

            if (currentPartnerIndex >= partnerCount) {
                currentPartnerIndex = 0;
            }
            partnerAddress = partners[currentPartnerIndex];
            if (!excludePartner[partnerAddress]) {
                
                REWARD_COIN.transferFrom(
                    address(_partnerRewardDistributor),
                    partnerAddress,
                    partnerRewardAmount
                );
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentPartnerIndex++;
            iterations++;
        }

        progressPartnerBlock = block.number;
    }

    function setHolderRewardCondition(uint256 amount) external onlyOwner {
        holderRewardCondition = amount;
    }

    function setSwapRewardCondition(uint256 amount) external onlyOwner {
        SwapRewardCondition = amount;
    }

    function setPartnerRewardCondition(uint256 amount) external onlyOwner {
        partnerRewardCondition = amount;
    }

    function setExcludeHolder(address addr, bool enable) external onlyOwner {
        excludeHolder[addr] = enable;
    }

    function setRewardHat(uint256 amount) external onlyOwner {
        rewardHat = amount;
    }
}