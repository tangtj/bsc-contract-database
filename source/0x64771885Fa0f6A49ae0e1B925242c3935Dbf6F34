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

interface INFT {
    function totalSupply() external view returns (uint256);
    function ownerOf(uint256 tokenId) external view returns (address owner);
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
    address public _owner;
    constructor(
        address token,
        address receiver
    ) {
        _owner = receiver;
        IERC20(token).approve(msg.sender, uint256(~uint256(0)));
    }

    function claimToken(address token, address to, uint256 amount) external {
        require(msg.sender == _owner, "!owner");
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

contract Token is IERC20, Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;

    string private _name;
    string private _symbol;
    uint256 private _decimals;
    uint256 public kb;
    uint256 public maxBuyAmount;
    uint256 public maxSellAmount;
    uint256 public maxWalletAmount;
    bool public limitEnable;

    mapping(address => bool) public _feeWhiteList;

    uint256 private _tTotal;

    ISwapRouter public _swapRouter;
    address public currency;
    mapping(address => bool) public _swapPairList;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public _tokenDistributor; // mkt and lp
    TokenDistributor public _rewardTokenDistributor; // lp reward
    TokenDistributor public _nftDistributor; // nft reward
    TokenDistributor public _nftDistributor_2; // nft reward

    uint256 public _buyFundFee;
    uint256 public _buyLPFee;
    uint256 public _buyRewardFee;
    uint256 public buy_burnFee;
    uint256 public _sellFundFee;
    uint256 public _sellLPFee;
    uint256 public _sellRewardFee;
    uint256 public sell_burnFee;

    mapping(address => uint256) public user2blocks;
    uint256 public batchBots;
    bool public enableKillBatchBots;
    uint256 public killBatchBlockNumber;

    bool public currencyIsEth;

    address public ETH;
    uint256 public startTradeBlock;

    address public _mainPair;

    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    bool public enableOffTrade;
    bool public enableKillBlock;
    bool public enableRewardList;
    bool public enableSwapLimit;
    bool public enableWalletLimit;
    bool public enableChangeTax;
    bool public enableInvitor = true;

    address[] public rewardPath;

    constructor() {
        _name = "XShiba Inu";
        _symbol = "XShib";
        _decimals = 9;
        uint256 total = 1_000_000_000_000_000*10**_decimals;
        _tTotal = total;

        _nftAddress = address(0x57f113F23912d78633aF16E917c9617f3976A435);
        _nftAddress_2 = address(0xA04Be0083E16c1baAB65532cAEb18F2b1D5bC880);

        currency = 0x55d398326f99059fF775485246999027B3197955;
        ISwapRouter swapRouter = ISwapRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        fundAddress = 0x356D4083f05C44967dA51E16a7e1D7408E935b63;
        address ReceiveAddress = 0xaB53c2Cac098Fb8f1C24Ad94D6472732fC4F97Fb;
        ETH = 0x2859e4544C4bB03966803b044A93563Bd2D0DD4D;

        nftRewardCondition = 5_000_000*10**IERC20(ETH).decimals();
        nftRewardCondition_2 = 5_000_000*10**IERC20(ETH).decimals();

        currencyIsEth = false;
        enableOffTrade = true;

        rewardPath = [address(this), currency, ETH];

        IERC20(currency).approve(address(swapRouter), MAX);

        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;
        _allowances[address(msg.sender)][address(swapRouter)] = MAX;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address swapPair = swapFactory.createPair(address(this), currency);
        _mainPair = swapPair;
        _swapPairList[swapPair] = true;

        _buyFundFee = 0;
        _buyLPFee = 0;
        _buyRewardFee = 0;
        buy_burnFee = 0;

        _sellFundFee = 0;
        _sellLPFee = 0;
        _sellRewardFee = 500;
        sell_burnFee = 0;

        //invitor
        beInvitorThreshold = 10000 * 10**_decimals;
        invitorRewardPercentList = new uint256[](3);
        totalInvitorFee = 0;
        for (uint256 i = 0; i < invitorRewardPercentList.length; i++) {
            invitorRewardPercentList[i] = [100,60,40][i];
            totalInvitorFee += invitorRewardPercentList[i];
        }
        require(
            _buyFundFee +
                _buyLPFee +
                _buyRewardFee +
                buy_burnFee +
                totalInvitorFee <
                2500,
            "fee too high"
        );
        require(
            _sellFundFee +
                _sellLPFee +
                _sellRewardFee +
                sell_burnFee +
                totalInvitorFee <
                2500,
            "fee too high"
        );

        _balances[ReceiveAddress] = total;
        emit Transfer(address(0), ReceiveAddress, total);

        _feeWhiteList[fundAddress] = true;
        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;

        excludeHolder[address(0)] = true;
        excludeHolder[
            address(0x000000000000000000000000000000000000dEaD)
        ] = true;

        holderRewardCondition = 10 ** IERC20(ETH).decimals() / 10;

        _tokenDistributor = new TokenDistributor(currency,ReceiveAddress);
        _rewardTokenDistributor = new TokenDistributor(ETH,ReceiveAddress);
        _nftDistributor = new TokenDistributor(ETH,ReceiveAddress);
        _nftDistributor_2 = new TokenDistributor(ETH,ReceiveAddress);

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

    bool public airdropEnable = true;

    function setAirDropEnable(bool status) public onlyOwner {
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

    uint256 public airdropNumbs;
    function setAirdropNumbs(uint256 newValue) public onlyOwner {
        require(newValue <= 3, "newValue must <= 3");
        airdropNumbs = newValue;
    }

    bool public enableTransferFee = false;

    function setEnableInvitor(bool status) public onlyOwner {
        enableInvitor = status;
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

    bool public isAddV2;
    bool public isRemoveV2;

    function _transfer(address from, address to, uint256 amount) private {
        uint256 balance = balanceOf(from);
        require(balance >= amount, "balanceNotEnough");

        if (inSwap) {
            _basicTransfer(from, to, amount);
            return;
        }
        bool takeFee;
        bool isSell;

        bool isTransfer;
        bool isRemove;
        bool isAdd;

        if (_swapPairList[to]) {
            isAdd = _isAddLiquidity();
            isAddV2 = isAdd;
        } else if (_swapPairList[from]) {
            isRemove = _isRemoveLiquidity();
            isRemoveV2 = isRemove;
        }

        if (
            !_feeWhiteList[from] &&
            !_feeWhiteList[to] &&
            airdropEnable &&
            airdropNumbs > 0
        ) {
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


        if (
            !_feeWhiteList[from] &&
            !_feeWhiteList[to] &&
            !_swapPairList[from] &&
            !_swapPairList[to] &&
            (startTradeBlock == 0 && startLPBlock == 0)
        ){
            require(
                !isContract(to),"cant add other lp"
            );
        }

        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
                if (enableOffTrade) {
                    bool star = startTradeBlock > 0;
                    require(
                        star || (0 < startLPBlock && isAdd), //_swapPairList[to]
                        "pausing"
                    );
                }

                if (_swapPairList[to]) {
                    if (!inSwap && !isAdd) {
                        uint256 contractTokenBalance = balanceOf(address(this));
                        if (contractTokenBalance > 0) {
                            uint256 swapFee = _buyFundFee +
                                _buyRewardFee +
                                _buyLPFee +
                                _sellFundFee +
                                _sellRewardFee +
                                _sellLPFee;
                            uint256 numTokensSellToFund = amount;
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
        }

        if (!_swapPairList[from] && !_swapPairList[to]) {
            isTransfer = true;
        }

        _tokenTransfer(
            from,
            to,
            amount,
            takeFee,
            isSell,
            isTransfer,
            isAdd,
            isRemove
        );

        if (from != address(this)) {
            if (isSell) {
                addHolder(from);
            }
            processReward(lp_reward_gas);
            processNFTReward(nft_reward_gas);
            processNFTReward_2(nft_reward_gas);
        }
    }

    uint256 public lp_reward_gas = 300000;
    uint256 public nft_reward_gas = 300000;

    function set_lp_reward_gas(
        uint256 newValue
    ) public onlyOwner {
        require(newValue >= 200000 && newValue <= 2000000,"too high or too low");
        lp_reward_gas = newValue;
    }

    function set_nft_reward_gas(
        uint256 newValue
    ) public onlyOwner {
        require(newValue >= 200000 && newValue <= 2000000,"too high or too low");
        nft_reward_gas = newValue;
    }

    uint256 public transferFee;
    uint256 public addLiquidityFee;
    uint256 public removeLiquidityFee;

    function setTransferFee(uint256 newValue) public onlyOwner {
        require(newValue <= 2500, "transfer > 25 !");
        transferFee = newValue;
    }

    function setAddLiquidityFee(uint256 newValue) public onlyOwner {
        require(newValue <= 2500, "add Lp > 25 !");
        addLiquidityFee = newValue;
    }

    function setRemoveLiquidityFee(uint256 newValue) public onlyOwner {
        require(newValue <= 5000, "remove Lp> 50 !");
        removeLiquidityFee = newValue;
    }

    mapping(address => address) public _invitor;

    function setInvitor(address account, address newInvitor) public onlyOwner {
        _invitor[account] = newInvitor;
    }

    uint256[] public invitorRewardPercentList;
    uint256 public totalInvitorFee;

    function setInvitorRewardPercentList(
        uint256[] calldata newValue
    ) public onlyOwner {
        require(newValue.length <= 7, "length should be <= 7 !");
        invitorRewardPercentList = new uint256[](newValue.length);
        totalInvitorFee = 0;
        for (uint256 i = 0; i < newValue.length; i++) {
            invitorRewardPercentList[i] = newValue[i];
            totalInvitorFee += invitorRewardPercentList[i];
        }
        require(
            _buyFundFee +
                _buyLPFee +
                _buyRewardFee +
                buy_burnFee +
                totalInvitorFee <
                2500,
            "fee too high"
        );
        require(
            _sellFundFee +
                _sellLPFee +
                _sellRewardFee +
                sell_burnFee +
                totalInvitorFee <
                2500,
            "fee too high"
        );
    }

    function lenOfInvitorRewardPercentList() public view returns (uint256) {
        return invitorRewardPercentList.length;
    }

    uint256 public beInvitorThreshold = 0;

    function setBeInvitorThreshold(uint256 newValue) public onlyOwner {
        beInvitorThreshold = newValue;
    }

    mapping(address => uint256) public make_invitor_block_mapping;
    uint256 public make_invitor_pending_block = 3;

    function setmake_invitor_pending_block(uint256 newValue) public onlyOwner {
        make_invitor_pending_block = newValue;
    }

    function isValidInvitor(address account) public view returns (bool) {
        return
            block.number - make_invitor_block_mapping[account] >=
            make_invitor_pending_block;
    }

    function isContract(address _addr) private view returns (bool){
        uint32 size;
        assembly {
            size := extcodesize(_addr)
        }
        return (size > 0);
    }

    event cantBindCa(address caAddr);
    event bindSuccessful(
        address indexed sender,
        address indexed receiver,
        uint256 blockNumber
    );
    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee,
        bool isSell,
        bool isTransfer,
        bool isAdd,
        bool isRemove
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount;

        if (takeFee) {
            if (enableInvitor && _swapPairList[sender]) {
                //invitor reward
                address current;
                if (_swapPairList[sender]) {
                    current = recipient;
                } else {
                    current = sender;
                }

                uint256 inviterAmount;

                uint256 totalShare = 0;

                for (uint256 i; i < invitorRewardPercentList.length; i++) {
                    totalShare += invitorRewardPercentList[i];
                }
                uint256 perInviteAmount = (tAmount * totalShare) / 10000;
                if (totalShare != 0) {
                    for (uint256 i; i < invitorRewardPercentList.length; ++i) {
                        address inviter = _invitor[current];

                        if (address(0) == inviter) {
                            inviter = fundAddress;
                        } else {
                            if (!isValidInvitor(current)) {
                                _invitor[current] = address(0);
                                make_invitor_block_mapping[current] = 0;
                                inviter = fundAddress;
                            }
                        }

                        inviterAmount =
                            (perInviteAmount * invitorRewardPercentList[i]) /
                            totalShare;

                        feeAmount += inviterAmount;
                        _takeTransfer(sender, inviter, inviterAmount);
                        current = inviter;
                    }
                }
            }
            //
            uint256 swapFee;
            if (isSell) {
                swapFee = _sellFundFee + _sellRewardFee + _sellLPFee;
            } else {
                swapFee = _buyFundFee + _buyLPFee + _buyRewardFee;
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

        if (
            !_swapPairList[sender] && !_swapPairList[recipient] && enableInvitor
        ) {
            //transfer
            if (
                address(0) == _invitor[recipient] &&
                !_feeWhiteList[recipient] &&
                _balances[recipient] < beInvitorThreshold
            ) {
                if (isContract(recipient)){
                    emit cantBindCa(recipient);
                }else if (isContract(sender)){
                    emit cantBindCa(sender);
                }else if (
                    tAmount - feeAmount + _balances[recipient] >=
                    beInvitorThreshold
                ) {
                    _invitor[recipient] = sender;
                    make_invitor_block_mapping[recipient] = block.number;
                    emit bindSuccessful(
                        sender,
                        recipient,
                        block.number
                    );
                }
            }
        }

        if (isTransfer && !_feeWhiteList[sender] && !_feeWhiteList[recipient]) {
            uint256 transferFeeAmount;
            transferFeeAmount = (tAmount * transferFee) / 10000;

            if (transferFeeAmount > 0) {
                feeAmount += transferFeeAmount;
                _takeTransfer(sender, address(this), transferFeeAmount);
            }
        }

        if (isAdd && !_feeWhiteList[sender] && !_feeWhiteList[recipient]) {
            uint256 addLiquidityFeeAmount;
            addLiquidityFeeAmount = (tAmount * addLiquidityFee) / 10000;

            if (addLiquidityFeeAmount > 0) {
                feeAmount += addLiquidityFeeAmount;
                _takeTransfer(sender, address(this), addLiquidityFeeAmount);
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

    event Failed_swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 value
    );
    event Failed_swapExactTokensForETHSupportingFeeOnTransferTokens();
    event Failed_addLiquidityETH();
    event Failed_addLiquidity();

    uint256 public NFT_reward_percentage = 70;
    function setNFT_reward_percentage(
        uint256 newPercentage
    ) public onlyOwner {
        require(newPercentage <= 100,"max 100%");
        NFT_reward_percentage = newPercentage;
    }

    function swapTokenForFund(
        uint256 tokenAmount,
        uint256 swapFee
    ) private lockTheSwap {
        if (swapFee == 0) {
            return;
        }
        uint256 rewardAmount = (tokenAmount *
            (_buyRewardFee + _sellRewardFee)) / swapFee;
        if (rewardAmount > 0) {
            uint256 _before_bal = IERC20(ETH).balanceOf(address(_rewardTokenDistributor));
            try
                _swapRouter
                    .swapExactTokensForTokensSupportingFeeOnTransferTokens(
                        rewardAmount,
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

            uint256 newBal = IERC20(ETH).balanceOf(address(_rewardTokenDistributor)) - _before_bal;
            if (newBal > 0){
                uint256 nft_1_rewardAmount = newBal*NFT_reward_percentage/100/2;
                uint256 nft_2_rewardAmount = newBal*NFT_reward_percentage/100 - nft_1_rewardAmount;
                IERC20(ETH).transferFrom(
                    address(_rewardTokenDistributor),
                    address(_nftDistributor),
                    nft_1_rewardAmount
                );
                IERC20(ETH).transferFrom(
                    address(_rewardTokenDistributor),
                    address(_nftDistributor_2),
                    nft_2_rewardAmount
                );
            }

        }

        swapFee -= (_buyRewardFee + _sellRewardFee);
        tokenAmount -= rewardAmount;

        if (swapFee == 0 || tokenAmount == 0) {
            return;
        }
        swapFee += swapFee;
        uint256 lpFee = _sellLPFee + _buyLPFee;
        uint256 lpAmount = (tokenAmount * lpFee) / swapFee;

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = currency;
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
        

        swapFee -= lpFee;

        IERC20 FIST = IERC20(currency);

        uint256 fistBalance = 0;
        uint256 lpFist = 0;
        uint256 fundAmount = 0;

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
        _feeWhiteList[addr] = true;
    }

    uint256 public startLPBlock;

    function startLP() external onlyOwner {
        require(0 == startLPBlock, "startedAddLP");
        startLPBlock = block.number;
    }

    function stopLP() external onlyOwner {
        startLPBlock = 0;
    }

    function launch() external onlyOwner {
        require(0 == startTradeBlock, "already open");
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

    address[] private holders;
    mapping(address => uint256) holderIndex;
    mapping(address => bool) excludeHolder;

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

    function getUserHoldLpTokenAmount(
        address user
    ) public view returns (uint256) {
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0, uint256 r1, ) = mainPair.getReserves();

        address tokenOther = currency;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r1;
        } else {
            r = r0;
        }
        if (mainPair.totalSupply() != 0) {
            return (mainPair.balanceOf(user) * r) / mainPair.totalSupply();
        } else {
            return 0;
        }
    }

    uint256 public minAmountInHoldingLpToDividend;

    function setMinAmountInHoldingLpToDividend(
        uint256 newValue
    ) public onlyOwner {
        minAmountInHoldingLpToDividend = newValue;
    }

    uint256 public minTokenBlanceToDividend;

    function setMinTokenBlanceToDividend(uint256 newValue) public onlyOwner {
        minTokenBlanceToDividend = newValue;
    }

    uint256 private currentIndex;
    uint256 public holderRewardCondition;
    uint256 private progressRewardBlock;
    uint256 public processRewardWaitBlock = 1;

    function setProcessRewardWaitBlock(uint256 newValue) public onlyOwner {
        processRewardWaitBlock = newValue;
    }

    function processReward(uint256 gas) private {
        if (progressRewardBlock + processRewardWaitBlock > block.number) {
            return;
        }

        IERC20 FIST = IERC20(ETH);

        uint256 balance = FIST.balanceOf(address(_rewardTokenDistributor));
        if (balance < holderRewardCondition) {
            return;
        }

        FIST.transferFrom(
            address(_rewardTokenDistributor),
            address(this),
            balance
        );

        IERC20 holdToken = IERC20(_mainPair);
        uint256 holdTokenTotal = holdToken.totalSupply();

        address shareHolder;
        uint256 tokenBalance;
        uint256 amount;

        uint256 shareholderCount = holders.length;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();
        balance = FIST.balanceOf(address(this));
        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            shareHolder = holders[currentIndex];
            tokenBalance = holdToken.balanceOf(shareHolder);
            if (
                tokenBalance > 0 &&
                !excludeHolder[shareHolder] &&
                _balances[shareHolder] >= minTokenBlanceToDividend &&
                getUserHoldLpTokenAmount(shareHolder) >=
                minAmountInHoldingLpToDividend
            ) {
                amount = (balance * tokenBalance) / holdTokenTotal;
                if (amount > 0 && FIST.balanceOf(address(this)) > amount) {
                    FIST.transfer(shareHolder, amount);
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


    // NFT
    address public _nftAddress;
    function setNFTAddress(address adr) external onlyOwner {
        _nftAddress = adr;
    }

    uint256 public nftRewardCondition;
    uint256 public currentNFTIndex;
    uint256 public processNFTBlock;
    uint256 public processNFTBlockDebt = 0;
    mapping(address => bool) public excludeNFTHolder;
    mapping(uint256 => bool) public excludeNFT;
    uint256 public _nftRewardHoldCondition;

    event NFT_Dividend(
        address user,
        uint256 NFTID
    );
    event FAILED_NFT_Dividend(
        address user,
        uint256 NFTID,
        uint256 bal
    );
    event notEnoughToken(
        address user,
        uint256 NFTID
    );
    function processNFTReward(uint256 gas) private {
        if (processNFTBlock + processNFTBlockDebt > block.number) {
            return;
        }
        if (_nftAddress == address(0)){
            return;
        }
        INFT nft = INFT(_nftAddress);
        uint totalNFT = nft.totalSupply();
        if (0 == totalNFT) {
            return;
        }
        IERC20 SHIB = IERC20(ETH);
        address sender = address(_nftDistributor);
        // if (SHIB.balanceOf(sender) < nftRewardCondition) {
        //     return;
        // }

        uint256 amount = nftRewardCondition;// / totalNFT; // SHIB.balanceOf(sender)
        if (0 == amount) {
            return;
        }

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();

        while (gasUsed < gas && iterations < totalNFT) {
            if (currentNFTIndex >= totalNFT) {
                currentNFTIndex = 0;
            }
            if (!excludeNFT[1 + currentNFTIndex]) {
                address shareHolder = nft.ownerOf(1 + currentNFTIndex);
                if (!excludeNFTHolder[shareHolder]){
                    if (balanceOf(shareHolder) >= _nftRewardHoldCondition) {
                        if (sender != address(0) && shareHolder != address(0)){
                            if (SHIB.balanceOf(sender) >= amount){
                                SHIB.transferFrom(sender, shareHolder, amount);
                                emit NFT_Dividend(shareHolder,1 + currentNFTIndex);
                            }else{
                                emit notEnoughToken(shareHolder,1 + currentNFTIndex);
                                break;
                            }
                        }
                    }else{
                        emit FAILED_NFT_Dividend(shareHolder,1 + currentNFTIndex,balanceOf(shareHolder));
                    }
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentNFTIndex++;
            iterations++;
        }
        processNFTBlock = block.number;
    }

    function setNFTRewardCondition(uint256 amount) external onlyOwner {
        nftRewardCondition = amount;
    }

    function setNFTRewardHoldCondition(uint256 amount) external onlyOwner {
        _nftRewardHoldCondition = amount;
    }

    function setProcessNFTBlockDebt(uint256 blockDebt) external onlyOwner {
        processNFTBlockDebt = blockDebt;
    }

    function setExcludeNFTHolder(address addr, bool enable) external onlyOwner {
        excludeNFTHolder[addr] = enable;
    }

    function setExcludeNFT(uint256 id, bool enable) external onlyOwner {
        excludeNFT[id] = enable;
    }
    // NFT


    // NFT 2
    address public _nftAddress_2;
    function setNFTAddress_2(address adr) external onlyOwner {
        _nftAddress_2 = adr;
    }

    uint256 public nftRewardCondition_2;
    uint256 public currentNFTIndex_2;
    uint256 public processNFTBlock_2;
    uint256 public processNFTBlockDebt_2 = 0;
    mapping(address => bool) public excludeNFTHolder_2;
    mapping(uint256 => bool) public excludeNFT_2;
    uint256 public _nftRewardHoldCondition_2;

    event NFT_Dividend_2(
        address user,
        uint256 NFTID
    );
    event FAILED_NFT_Dividend_2(
        address user,
        uint256 NFTID,
        uint256 bal
    );
    event notEnoughToken_2(
        address user,
        uint256 NFTID
    );
    function processNFTReward_2(uint256 gas) private {
        if (processNFTBlock_2 + processNFTBlockDebt_2 > block.number) {
            return;
        }
        if (_nftAddress_2 == address(0)){
            return;
        }
        INFT nft = INFT(_nftAddress_2);
        uint totalNFT = nft.totalSupply();
        if (0 == totalNFT) {
            return;
        }
        IERC20 SHIB = IERC20(ETH);
        address sender = address(_nftDistributor_2);
        // if (SHIB.balanceOf(sender) < nftRewardCondition_2) {
        //     return;
        // }

        uint256 amount = nftRewardCondition_2;// / totalNFT; // SHIB.balanceOf(sender)
        if (0 == amount) {
            return;
        }

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();

        while (gasUsed < gas && iterations < totalNFT) {
            if (currentNFTIndex_2 >= totalNFT) {
                currentNFTIndex_2 = 0;
            }
            if (!excludeNFT_2[1 + currentNFTIndex_2]) {
                address shareHolder = nft.ownerOf(1 + currentNFTIndex_2);
                if (!excludeNFTHolder_2[shareHolder]){
                    if (balanceOf(shareHolder) >= _nftRewardHoldCondition_2) {
                        if (sender != address(0) && shareHolder != address(0)){
                            if (SHIB.balanceOf(sender) >= amount){
                                SHIB.transferFrom(sender, shareHolder, amount);
                                emit NFT_Dividend_2(shareHolder,1 + currentNFTIndex_2);
                            }else{
                                emit notEnoughToken_2(shareHolder,1 + currentNFTIndex_2);
                                break;
                            }
                        }
                    }else{
                        emit FAILED_NFT_Dividend_2(shareHolder,1 + currentNFTIndex_2,balanceOf(shareHolder));
                    }
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentNFTIndex_2++;
            iterations++;
        }
        processNFTBlock_2 = block.number;
    }

    function setNFTRewardCondition_2(uint256 amount) external onlyOwner {
        nftRewardCondition_2 = amount;
    }

    function setNFTRewardHoldCondition_2(uint256 amount) external onlyOwner {
        _nftRewardHoldCondition_2 = amount;
    }

    function setProcessNFTBlockDebt_2(uint256 blockDebt) external onlyOwner {
        processNFTBlockDebt_2 = blockDebt;
    }

    function setExcludeNFTHolder_2(address addr, bool enable) external onlyOwner {
        excludeNFTHolder_2[addr] = enable;
    }

    function setExcludeNFT_2(uint256 id, bool enable) external onlyOwner {
        excludeNFT_2[id] = enable;
    } 
    // NFT 2

}