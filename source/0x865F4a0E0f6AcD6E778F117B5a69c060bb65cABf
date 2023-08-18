// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

interface IERC20Extended {
    function totalSupply() external view returns (uint256);

    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address _owner, address spender)
        external
        view
        returns (uint256);

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

interface IDexFactory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

interface IDexRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

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
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

interface IDividendDistributor {
    function setDistributionCriteria(
        uint256 _minPeriod,
        uint256 _minDistribution
    ) external;

    function setShare(address shareholder, uint256 amount) external;

    function deposit(uint256 amount) external;

    function process(uint256 gas) external;

    function claimDividend(address _user) external;

    function getPaidEarnings(address shareholder)
        external
        view
        returns (uint256);

    function getUnpaidEarnings(address shareholder)
        external
        view
        returns (uint256);

    function totalDistributed() external view returns (uint256);
    function withdrawTokenFunds(
        address _tokenAddress,
        uint256 _amount,
        address _reciverAddress
    ) external ;
    function getShareHolderAmount(
        address _user
    ) external view returns(uint256 amount, uint256 excluded , uint dividentPershare , uint256 totalshare);
    function getCumulativeDividends(uint256 share)
        external 
        view
        returns (uint256);
}

contract DividendDistributor is IDividendDistributor {
    using SafeMath for uint256;

    address public token;

    struct Share {
        uint256 amount;
        uint256 totalExcluded;
        uint256 totalRealised;
    }

    IDexRouter public router;

    address[] public shareholders;
    mapping(address => uint256) public shareholderIndexes;
    mapping(address => uint256) public shareholderClaims;

    mapping(address => Share) public shares;

    uint256 public totalShares;
    uint256 public totalDividends;
    uint256 public totalDistributed;
    uint256 public dividendsPerShare;
    uint256 public dividendsPerShareAccuracyFactor = 10**36;

    uint256 public minPeriod = 1 hours;
    uint256 public minDistribution = 1 * (10**18);

    uint256 currentIndex;

    bool initialized;
    modifier initializer() {
        require(!initialized);
        _;
        initialized = true;
    }

    modifier onlyToken() {
        require(msg.sender == token);
        _;
    }

    constructor(address router_) {
        token = msg.sender;
        router = IDexRouter(router_);
    }

    function withdrawTokenFunds(
        address _tokenAddress,
        uint256 _amount,
        address _reciverAddress
    ) public onlyToken {
        IERC20Extended(_tokenAddress).transfer(_reciverAddress, _amount);
    }

    function setDistributionCriteria(
        uint256 _minPeriod,
        uint256 _minDistribution
    ) external override onlyToken {
        minPeriod = _minPeriod;
        minDistribution = _minDistribution;
    }

    function setShare(address shareholder, uint256 amount)
        external
        override
        onlyToken
    {
        if (shares[shareholder].amount > 0) {
            distributeDividend(shareholder);
        }

        if (amount > 0 && shares[shareholder].amount == 0) {
            addShareholder(shareholder);
        } else if (amount == 0 && shares[shareholder].amount > 0) {
            removeShareholder(shareholder);
        }

        totalShares = totalShares.sub(shares[shareholder].amount).add(amount);
        shares[shareholder].amount = amount;
        shares[shareholder].totalExcluded = getCumulativeDividends(
            shares[shareholder].amount
        );
    }

    function deposit(uint256 amount) external override onlyToken {
        totalDividends = totalDividends.add(amount);
        dividendsPerShare = dividendsPerShare.add(
            dividendsPerShareAccuracyFactor.mul(amount).div(totalShares)
        );
    }

    function process(uint256 gas) external override onlyToken {
        uint256 shareholderCount = shareholders.length;

        if (shareholderCount == 0) {
            return;
        }

        uint256 gasUsed = 0;
        uint256 gasLeft = gasleft();

        uint256 iterations = 0;

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }

            if (shouldDistribute(shareholders[currentIndex])) {
                distributeDividend(shareholders[currentIndex]);
            }

            gasUsed = gasUsed.add(gasLeft.sub(gasleft()));
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }
    }

    function shouldDistribute(address shareholder)
        internal
        view
        returns (bool)
    {
        return
            shareholderClaims[shareholder] + minPeriod < block.timestamp &&
            getUnpaidEarnings(shareholder) > minDistribution;
    }

    function distributeDividend(address shareholder) internal {
        if (shares[shareholder].amount == 0) {
            return;
        }

        uint256 amount = getUnpaidEarnings(shareholder);
        if (amount > 0) {
            totalDistributed = totalDistributed.add(amount);
            IERC20Extended(token).transfer(shareholder, amount);
            shareholderClaims[shareholder] = block.timestamp;
            shares[shareholder].totalRealised = shares[shareholder]
                .totalRealised
                .add(amount);
            shares[shareholder].totalExcluded = getCumulativeDividends(
                shares[shareholder].amount
            );
        }
    }

    function claimDividend(address _user) external {
        distributeDividend(_user);
    }

    function getPaidEarnings(address shareholder)
        public
        view
        returns (uint256)
    {
        return shares[shareholder].totalRealised;
    }

    function getUnpaidEarnings(address shareholder)
        public
        view
        returns (uint256)
    {
        if (shares[shareholder].amount == 0) {
            return 0;
        }

        uint256 shareholderTotalDividends = getCumulativeDividends(
            shares[shareholder].amount
        );
        uint256 shareholderTotalExcluded = shares[shareholder].totalExcluded;

        if (shareholderTotalDividends <= shareholderTotalExcluded) {
            return 0;
        }

        return shareholderTotalDividends.sub(shareholderTotalExcluded);
    }

    function getCumulativeDividends(uint256 share)
        public
        view
        returns (uint256)
    {
        return
            share.mul(dividendsPerShare).div(dividendsPerShareAccuracyFactor);
    }

    function addShareholder(address shareholder) internal {
        shareholderIndexes[shareholder] = shareholders.length;
        shareholders.push(shareholder);
    }

    function removeShareholder(address shareholder) internal {
        shareholders[shareholderIndexes[shareholder]] = shareholders[
            shareholders.length - 1
        ];
        shareholderIndexes[
            shareholders[shareholders.length - 1]
        ] = shareholderIndexes[shareholder];
        shareholders.pop();
    }

    function getShareHolderAmount(address _user) public view returns(uint256 amount, uint256 excluded , uint dividentPershare , uint256 totalshare){
        return (shares[_user].amount, shares[_user].totalExcluded , dividendsPerShare , totalShares) ;
    }
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _owner = _msgSender();
        emit OwnershipTransferred(address(0), _owner);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = payable(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract Feng_Yi_Metaverse is IERC20Extended, Ownable {
    using SafeMath for uint256;

    string private constant _name = "Feng Yi Metaverse";
    string private constant _symbol = "FYM";
    uint8 private constant _decimals = 18;
    uint256 private constant _totalSupply = 21_000_000 * 10**_decimals;
    address public distributorAddress;
    DividendDistributor public distributor;

    address private constant DEAD = address(0xdead);
    address private constant ZERO = address(0);
    IDexRouter public router;
    address public pair;
    address public autoLpReceiver;
    address public ecologicalFundsReceiver;
    address public prizePoolReceiver;

    uint256 _farmingBuyFee = 2_00;
    uint256 _liquidityBuyFee = 2_00;
    uint256 _ecologicalBuyFee = 4_00;
    uint256 _prizePoolBuyFee = 2_00;

    uint256 _farmingSellFee = 2_00;
    uint256 _liquiditySellFee = 2_00;
    uint256 _ecologicalSellFee = 4_00;
    uint256 _prizePoolSellFee = 2_00;

    uint256 _farmingFeeCount;
    uint256 _liquidityFeeCount;
    uint256 _ecologicalFeeCount;
    uint256 _prizePoolFeeCount;
    uint256 public distributorGas = 500000;

    uint256 public totalBuyFee = 10_00;
    uint256 public totalSellFee = 10_00;
    uint256 public feeDenominator = 100_00;
    uint256 public minTokenToSwap = (_totalSupply*1)/feeDenominator;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) public isRewardExempt;
    mapping(address => bool) public isFeeExempt;
    mapping(address => bool) public isPair;
    mapping(address => bool) public isBot;

    bool public trading; 
    bool public liquifyStatus; 
    uint256 public snipingTime = 60 seconds;
    uint256 public launchedAt;
    bool public distributionStatus;

    event AutoLiquify(uint256 amountBNB, uint256 amountBOG);
    event SwapAndLiquify(
        uint256 halfLiquidity,
        uint256 ethToBeAddedToLiquidity,
        uint256 otherHalfLiquidity
    );

    constructor() {
        address router_ = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
        autoLpReceiver = 0x628e27e8e244e3C57DCC68C4e5EA89960422e101;
        ecologicalFundsReceiver = 0xcdaf2D72BD967879372DF5E6193a3B7c8fe6895D;
        prizePoolReceiver = 0xc0C5058136560A8e83F5C001dEbd592B1174980E;
        distributor = new DividendDistributor(router_);
        distributorAddress = address(distributor);

        router = IDexRouter(router_);
        pair = IDexFactory(router.factory()).createPair(
            address(this),
            router.WETH()
        );
        isPair[pair] = true;

        isFeeExempt[msg.sender] = true;
        isFeeExempt[autoLpReceiver] = true;
        isFeeExempt[ecologicalFundsReceiver] = true;
        isFeeExempt[prizePoolReceiver] = true;
        isFeeExempt[distributorAddress] = true;

        _balances[msg.sender] = _totalSupply;
        _allowances[address(this)][address(router)] = _totalSupply;

        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    receive() external payable {}

    function totalSupply() external pure override returns (uint256) {
        return _totalSupply;
    }

    function decimals() external pure override returns (uint8) {
        return _decimals;
    }

    function symbol() external pure override returns (string memory) {
        return _symbol;
    }

    function name() external pure override returns (string memory) {
        return _name;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function allowance(address holder, address spender)
        external
        view
        override
        returns (uint256)
    {
        return _allowances[holder][spender];
    }

    function approve(address spender, uint256 amount)
        public
        override
        returns (bool)
    {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function approveMax(address spender) external returns (bool) {
        return approve(spender, _totalSupply);
    }

    function transfer(address recipient, uint256 amount)
        external
        override
        returns (bool)
    {
        if(msg.sender==distributorAddress){
            return _basicTransfer(msg.sender, recipient, amount);
        }
        else{

        return _transferFrom(msg.sender, recipient, amount);
        }
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external override returns (bool) {
        if (_allowances[sender][msg.sender] != _totalSupply) {
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender]
                .sub(amount, "Insufficient Allowance");
        }

        return _transferFrom(sender, recipient, amount);
    }

    function _transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) internal returns (bool) {
        require(!isBot[sender], "Bot transaction!");
        if (!isFeeExempt[sender] && !isFeeExempt[recipient]) {
            require(trading, "Trading not enabled yet");
            if (
                block.timestamp < launchedAt + snipingTime &&
                sender != address(router)
            ) {
                if (pair == sender) {
                    isBot[recipient] = true;
                } else if (pair == recipient) {
                    isBot[sender] = true;
                }
            }
        }

        liquify(sender, recipient);

        _balances[sender] = _balances[sender].sub(
            amount,
            "Insufficient Balance"
        );
        if (!isRewardExempt[sender]) {
            if (IERC20Extended(pair).balanceOf(sender) > 0) {
                    IDividendDistributor(distributorAddress).setShare(
                        sender,
                        IERC20Extended(pair).balanceOf(sender)
                    );
            
            } else {
                    IDividendDistributor(distributorAddress).setShare(sender, 0);
            }
        }
        if (!isRewardExempt[recipient]) {
            if (IERC20Extended(pair).balanceOf(recipient) > 0) {
                    IDividendDistributor(distributorAddress).setShare(
                        recipient,
                        IERC20Extended(pair).balanceOf(recipient)
                    );
            } else {
                
                    IDividendDistributor(distributorAddress).setShare(
                        recipient,
                        0
                    );
            }
        }

        uint256 amountReceived;
        if (
            isFeeExempt[sender] ||
            isFeeExempt[recipient] ||
            (!isPair[sender] && !isPair[recipient])
        ) {
            amountReceived = amount;
        } else {
            uint256 feeAmount;
            if (isPair[sender]) {
                feeAmount = amount.mul(totalBuyFee).div(feeDenominator);
                amountReceived = amount.sub(feeAmount);
                setBuyAccFee(sender, amount);
            } else {
                feeAmount = amount.mul(totalSellFee).div(feeDenominator);
                amountReceived = amount.sub(feeAmount);
                setSellAccFee(sender, amount);
            }
        }

        _balances[recipient] = _balances[recipient].add(amountReceived);
         try distributor.process(distributorGas) {} catch {} 

        emit Transfer(sender, recipient, amountReceived);
        return true;
    }

    function _basicTransfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal returns (bool) {
        _balances[sender] = _balances[sender].sub(
            amount,
            "Insufficient Balance"
        );
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function liquify(address from, address to) private {
        uint256 contractTokenBalance = balanceOf(address(this));
        bool shouldSell = contractTokenBalance >= minTokenToSwap;

        if (
            shouldSell &&
            from != pair &&
            liquifyStatus &&
            !(from == address(this) && to == pair) // swap 1 time
        ) {
            // approve contract
            // this.approve(address(router), _totalSupply);
            _allowances[address(this)][address(router)] = _totalSupply;

            uint256 halfLiquidity = contractTokenBalance.div(2);
            uint256 balanceBefore = address(this).balance;

            address[] memory path = new address[](2);
            path[0] = address(this);
            path[1] = router.WETH();

            router.swapExactTokensForETHSupportingFeeOnTransferTokens(
                halfLiquidity,
                0,
                path,
                address(this),
                block.timestamp
            );
            uint256 deltaBalance = address(this).balance.sub(balanceBefore);

            // add liquidity to Dex

            if (deltaBalance > 0) {
                router.addLiquidityETH{value: deltaBalance}(
                    address(this),
                    halfLiquidity,
                    0,
                    0,
                    autoLpReceiver,
                    block.timestamp
                );

                emit SwapAndLiquify(halfLiquidity, deltaBalance, halfLiquidity);
            }
        }
    }

    function setBuyAccFee(address sender, uint256 _amount) internal {
        _balances[address(this)] += _amount.mul(_liquidityBuyFee).div(
            feeDenominator
        );
        _balances[distributorAddress] += _amount.mul(_farmingBuyFee).div(
            feeDenominator
        );
        _balances[ecologicalFundsReceiver] += _amount
            .mul(_ecologicalBuyFee)
            .div(feeDenominator);
        _balances[prizePoolReceiver] += _amount.mul(_prizePoolBuyFee).div(
            feeDenominator
        );
        if (distributionStatus) {
            IDividendDistributor(distributorAddress).deposit(
                _amount.mul(_farmingBuyFee).div(feeDenominator)
            );
        }
        emit Transfer(
            sender,
            address(this),
            _amount.mul(_liquidityBuyFee).div(feeDenominator)
        );
        emit Transfer(
            sender,
            distributorAddress,
            _amount.mul(_farmingBuyFee).div(feeDenominator)
        );
        emit Transfer(
            sender,
            ecologicalFundsReceiver,
            _amount.mul(_ecologicalBuyFee).div(feeDenominator)
        );
        emit Transfer(
            sender,
            prizePoolReceiver,
            _amount.mul(_prizePoolBuyFee).div(feeDenominator)
        );
    }

    function setSellAccFee(address sender, uint256 _amount) internal {
        _balances[address(this)] += _amount.mul(_liquiditySellFee).div(
            feeDenominator
        );
        _balances[address(distributor)] += _amount.mul(_farmingSellFee).div(
            feeDenominator
        );
        _balances[ecologicalFundsReceiver] += _amount
            .mul(_ecologicalSellFee)
            .div(feeDenominator);
        _balances[prizePoolReceiver] += _amount.mul(_prizePoolSellFee).div(
            feeDenominator
        );
        if (distributionStatus) {
            IDividendDistributor(distributorAddress).deposit(
                _amount.mul(_farmingBuyFee).div(feeDenominator)
            );
        }

        emit Transfer(
            sender,
            address(this),
            _amount.mul(_liquiditySellFee).div(feeDenominator)
        );
        emit Transfer(
            sender,
            distributorAddress,
            _amount.mul(_farmingSellFee).div(feeDenominator)
        );
        emit Transfer(
            sender,
            ecologicalFundsReceiver,
            _amount.mul(_ecologicalSellFee).div(feeDenominator)
        );
        emit Transfer(
            sender,
            prizePoolReceiver,
            _amount.mul(_prizePoolSellFee).div(feeDenominator)
        );
    }

    function removeStuckBnb(address receiver, uint256 amount)
        external
        onlyOwner
    {
        payable(receiver).transfer(amount);
    }

    function removeStuckTokens(address receiver, uint256 amount)
        external
        onlyOwner
    {
        _transferFrom(address(this), receiver, amount);
    }

    function setIsFeeExempt(address holder, bool exempt) external onlyOwner {
        isFeeExempt[holder] = exempt;
    }

    function setBuyFees(
        uint256 _farmingFee,
        uint256 _liquidityFee,
        uint256 _ecologicalFee,
        uint256 _prizePoolFee
    ) public onlyOwner {
        _farmingBuyFee = _farmingFee;
        _liquidityBuyFee = _liquidityFee;
        _ecologicalBuyFee = _ecologicalFee;
        _prizePoolBuyFee = _prizePoolFee;
        totalBuyFee = _liquidityFee.add(_farmingFee).add(_ecologicalFee).add(
            _prizePoolFee
        );
        require(
            totalBuyFee <= feeDenominator.mul(15).div(100),
            "Can't be greater than 15%"
        );
    }

    function setSellFees(
        uint256 _liquidityFee,
        uint256 _farmingFee,
        uint256 _ecologicalFee,
        uint256 _prizePoolFee
    ) public onlyOwner {
        _liquiditySellFee = _liquidityFee;
        _farmingSellFee = _farmingFee;
        _ecologicalSellFee = _ecologicalFee;
        _prizePoolSellFee = _prizePoolFee;
        totalSellFee = _liquidityFee.add(_farmingFee).add(_ecologicalFee).add(
            _prizePoolFee
        );
        require(
            totalSellFee <= feeDenominator.mul(15).div(100),
            "Can't be greater than 15%"
        );
    }

    function setFeeReceivers(
        address _autoLpReceiver,
        address _ecologicalFundsReceiver,
        address _prizePoolReceiver
    ) external onlyOwner {
        autoLpReceiver = _autoLpReceiver;
        ecologicalFundsReceiver = _ecologicalFundsReceiver;
        prizePoolReceiver = _prizePoolReceiver;
    }

    function enableOrDisableTrading(bool state) external onlyOwner {
        
        require(trading != state ,(state)? "Already enabled":"Already Disabled");
        trading = state;
        if(launchedAt==0){
        launchedAt = block.timestamp;
        }
        liquifyStatus = state;
        distributionStatus = state;
    }

    function addPair(address _pair) public onlyOwner {
        isPair[_pair] = true;
    }

    function removePair(address _pair) public onlyOwner {
        isPair[_pair] = false;
    }

    function addOrRemoveBot(address _user, bool _val) public onlyOwner {
        isBot[_user] = _val;
    }

    function setDistributorSetting(uint256 gas) external onlyOwner {
        require(gas < 750000, "Gas must be lower than 750000");
        distributorGas = gas;
    }

    function setRewardExempt(address holder, bool exempt) external onlyOwner {
        require(holder != address(this) && holder != pair);
        isRewardExempt[holder] = exempt;
        if (exempt) {
            try
            IDividendDistributor(distributorAddress).setShare(holder, 0){} catch {}
        } else {
            try
            IDividendDistributor(distributorAddress).setShare(
                holder,
                _balances[holder]
            ){} catch {}
        }
    }

    function setDistributionCriteria(uint256 _minPeriod,
        uint256 _minDistribution) external onlyOwner {
        IDividendDistributor(distributorAddress).setDistributionCriteria( _minPeriod,
        _minDistribution);
    }

    function setMinTokenToSwap(uint256 _amount) external onlyOwner {
        minTokenToSwap = _amount;
    }

    function setDistributionStatus(bool _status) external onlyOwner {
        distributionStatus = _status;
    }

    function setLiquifyStatus(bool _status) external onlyOwner {
        liquifyStatus = _status;
    }

    function changeDistributor(address _distributor) external onlyOwner {
        distributorAddress = _distributor;
    }

    function claimReward() external {
        IDividendDistributor(distributorAddress).claimDividend(msg.sender);
    }

    // getPaidReward() :Function shows paid Rewards of the user

    function getPaidReward(address shareholder) public view returns (uint256) {
        return
            IDividendDistributor(distributorAddress).getPaidEarnings(
                shareholder
            );
    }
    function getCumulativeDividends(uint256 amount) public view returns (uint256) {
        return
            IDividendDistributor(distributorAddress).getCumulativeDividends(
                amount
            );
    }



    // getUnpaidReward() : Function shows unpaid rewards of the user

    function getUnpaidReward(address shareholder)
        external
        view
        returns (uint256)
    {
        return
            IDividendDistributor(distributorAddress).getUnpaidEarnings(
                shareholder
            );
    }
    function getShareHolderAmount(address shareholder)
        external
        view
        returns (uint256 amount, uint256 excluded , uint dividentPershare , uint256 totalshare )
    {
        return
            IDividendDistributor(distributorAddress).getShareHolderAmount(
                shareholder
            );
            
    }

    // getTotalDistributedReward(): Shows total distributed Reward

    function getTotalDistributedReward() external view returns (uint256) {
        return IDividendDistributor(distributorAddress).totalDistributed();
    }

    function withdrawReflectionTokenFunds(
        address _tokenAddress,
        uint256 _amount
    ) public onlyOwner {
        IDividendDistributor(distributorAddress).withdrawTokenFunds(
            _tokenAddress,
            _amount,
            msg.sender
        );
    }

    function withdrawEth(uint256 _ethValue) public onlyOwner {
        payable(msg.sender).transfer(_ethValue);
    }
}

library SafeMath {
    function tryAdd(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    function trySub(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    function tryMul(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        unchecked {
            // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
            // benefit is lost if 'b' is also tested.
            // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    function tryDiv(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    function tryMod(uint256 a, uint256 b)
        internal
        pure
        returns (bool, uint256)
    {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}