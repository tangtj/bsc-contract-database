//SPDX-License-Identifier: MIT

// Telegram: https://t.me/dgirotoprofessional
// WebSite:  htps://dgiroto.com
// Twitter:  https://twitter.com/crypto_dgiroto
// Email:    contato@dgitoto.com

//Tax Buy:

// 2.5% Stake DGP win DGP on the da website: martik.site/stake
// 2.5% Reflection to holders in DGP
// 2.5% Burn : 0x000000000000000000000000000000000000dead
// 2.5% Reward in Martik: Smart Contract: 0x116526135380E28836C6080f1997645d5A807FAE

 //Tax Sell:

// 2.5% Stake DGP win DGP on the da website: martik.site/stake
// 2.5% Reflection to holders in DGP
// 2.5% Burn : 0x000000000000000000000000000000000000dead
// 2.5% Reward in Martik: Smart Contract: 0x116526135380E28836C6080f1997645d5A807FAE
// 9% in bnb bep20 for marketing portfolio

pragma solidity ^0.8.7;

interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
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

interface IRouter {
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

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;

    function getAmountsOut(uint256 amountIn, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);
}

interface IFactory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; 
        return msg.data;
    }
}

library Address {
    function sendValue(address payable recipient, uint256 amount) internal {
        require(
            address(this).balance >= amount,
            "Address: insufficient balance"
        );

        (bool success, ) = recipient.call{value: amount}("");
        require(
            success,
            "Address: unable to send value, recipient may have reverted"
        );
    }
}

contract DividendDistributor {
    address public _token;
    address public WBNB;
    address[] public shareholders;

    struct Share {
        uint256 amount;
        uint256 totalExcluded;
        uint256 totalRealised;
    }

    IBEP20 public REWARD;
    IRouter public router;

    mapping(address => uint256) public shareholderIndexes;
    mapping(address => uint256) public shareholderClaims;
    mapping(address => Share) public shares;

    uint256 public totalShares;
    uint256 public totalDividends;
    uint256 public totalDistributed;
    uint256 public dividendsPerShare;
    uint256 public dividendsPerShareAccuracyFactor = 10**36;
    uint256 public minPeriod = 1 hours;
    uint256 public minDistribution = 1 * (10**8);
    uint256 public currentIndex;
    bool initialized;
    modifier initialization() {
        require(!initialized);
        _;
        initialized = true;
    }

    modifier onlyToken() {
        require(msg.sender == _token);
        _;
    }

    constructor(
        address _router,
        IBEP20 reward,
        address token
    ) {
        REWARD = reward;
        router = IRouter(_router);
        _token = token;
        WBNB = router.WETH();
    }

    receive() external payable {}

    function setDistributionCriteria(
        uint256 _minPeriod,
        uint256 _minDistribution
    ) external onlyToken {
        minPeriod = _minPeriod;
        minDistribution = _minDistribution;
    }

    function setShare(address shareholder, uint256 amount) external onlyToken {
        if (shares[shareholder].amount > 0) {
            distributeDividend(shareholder);
        }
        if (amount > 0 && shares[shareholder].amount == 0) {
            addShareholder(shareholder);
        } else if (amount == 0 && shares[shareholder].amount > 0) {
            removeShareholder(shareholder);
        }
        totalShares = (totalShares - shares[shareholder].amount) + amount;
        shares[shareholder].amount = amount;
        shares[shareholder].totalExcluded = getCumulativeDividends(
            shares[shareholder].amount
        );
    }

    function deposit() external payable onlyToken {
        uint256 balanceBefore = REWARD.balanceOf(address(this));
        address[] memory path = new address[](2);
        path[0] = WBNB;
        path[1] = address(REWARD);
        uint256[] memory price = router.getAmountsOut(msg.value, path);
        router.swapExactETHForTokensSupportingFeeOnTransferTokens{
            value: msg.value
        }(0, path, address(this), block.timestamp);
        uint256 amount = REWARD.balanceOf(address(this)) - balanceBefore;
        totalDividends = totalDividends + amount;
        dividendsPerShare =
            dividendsPerShare +
            ((dividendsPerShareAccuracyFactor * amount) / totalShares);
    }

    function process(uint256 gas) external onlyToken {
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
            gasUsed = (gasUsed + gasLeft) - gasleft();
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
            totalDistributed = totalDistributed + amount;
            REWARD.transfer(shareholder, amount);
            shareholderClaims[shareholder] = block.timestamp;
            shares[shareholder].totalRealised =
                shares[shareholder].totalRealised +
                amount;
            shares[shareholder].totalExcluded = getCumulativeDividends(
                shares[shareholder].amount
            );
        }
    }

    function claimDividend(address shareholder) external onlyToken {
        distributeDividend(shareholder);
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
        return shareholderTotalDividends - shareholderTotalExcluded;
    }

    function getCumulativeDividends(uint256 share)
        internal
        view
        returns (uint256)
    {
        return (share * dividendsPerShare) / dividendsPerShareAccuracyFactor;
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

    function setDividendTokenAddress(address newToken) external onlyToken {
        REWARD = IBEP20(newToken);
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _setOwner(_msgSender());
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

contract DGirotoProfessional is Context, Ownable {
    using Address for address payable;
    //IMPORTS
    DividendDistributor public distributor;
    IBEP20 public MTK = IBEP20(0x116526135380E28836C6080f1997645d5A807FAE); 
    IRouter public router = IRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E); 
    //MAPPING
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => uint256) public _rOwned;
    mapping(address => uint256) public _tOwned;
    mapping(address => bool) private _isExcludedFromFee; 
    mapping(address => bool) private _isExcluded; 
    mapping(address => bool) public isDividendExempt;
    mapping(address => bool) public isPair;

    address[] private _excluded;
    address public deadWallet = 0x000000000000000000000000000000000000dEaD;
    address public marketingWallet = msg.sender;
    address public rewardWallet = msg.sender;
  
    bool public swapEnabled = true;
    bool private swapping;

    uint8 private constant _decimals = 18;
    uint256 private constant MAX = ~uint256(0);
    uint256 private _tTotal = 420000000000000 * 10**_decimals;
    uint256 private _rTotal = (MAX - (MAX % _tTotal));
    uint256 public distributorGas = 300000;
   
    string private constant _name = "D'Giroto Professional";
    string private constant _symbol = "DGP";
   
    struct Taxes {
        uint256 rfi;
        uint256 marketing;
        uint256 Burn;
        uint256 TokenRef;
    }
    struct TotFeesPaidStruct {
        uint256 rfi;
        uint256 marketing;
        uint256 Burn;
        uint256 TokenRef;
    }
    struct valuesFromGetValues {
        uint256 rAmount;
        uint256 rTransferAmount;
        uint256 rRfi;
        uint256 rBurn;
        uint256 rMarketing;
        uint256 rTokenRef;
        uint256 tTransferAmount;
        uint256 tRfi;
        uint256 tBurn;
        uint256 tMarketing;
        uint256 tTokenRef;
    }
    Taxes public taxes = Taxes(250, 250 , 250, 250);
    Taxes public sellTaxes = Taxes(25, 25, 925, 25);
    TotFeesPaidStruct public totFeesPaid;
   
    modifier lockTheSwap() {
        swapping = true;
        _;
        swapping = false;
    }

    constructor() {
        address _pair = IFactory(router.factory()).createPair(
            address(this),
            router.WETH()
        );

        distributor = new DividendDistributor(
            address(router),
            MTK,
            address(this)
        );

        excludeFromReward(_pair);
        excludeFromReward(address(0));
        excludeFromReward(address(this));
        _rOwned[owner()] = _rTotal;

        isPair[_pair] = true;

        isDividendExempt[_pair] = true;
        isDividendExempt[address(this)] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[marketingWallet] = true;
        _isExcludedFromFee[deadWallet] = true;

        emit Transfer(address(0), owner(), _tTotal);
    }

    receive() external payable {}

    function name() public pure returns (string memory) {
        return _name;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view returns (uint256) {
        if (_isExcluded[account]) {
            return _tOwned[account];
        } else {
            return tokenFromReflection(_rOwned[account]);
        }
    }

    function allowance(address owner, address spender)
        public
        view
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue)
        public
        returns (bool)
    {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender] + addedValue
        );
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        returns (bool)
    {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(
            currentAllowance >= subtractedValue,
            "BEP20: decreased allowance below zero"
        );
        _approve(_msgSender(), spender, currentAllowance - subtractedValue);

        return true;
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) private {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function isExcludedFromReward(address account) public view returns (bool) {
        return _isExcluded[account];
    }

    function isExcludedFromFee(address account) public view returns (bool) {
        return _isExcludedFromFee[account];
    }

    function reflectionFromToken(uint256 tAmount, bool deductTransferRfi)
        public
        view
        returns (uint256)
    {
        require(tAmount <= _tTotal, "Amount must be less than supply");
        if (!deductTransferRfi) {
            valuesFromGetValues memory s = _getValues(tAmount, true, false);
            return s.rAmount;
        } else {
            valuesFromGetValues memory s = _getValues(tAmount, true, false);
            return s.rTransferAmount;
        }
    }

    function tokenFromReflection(uint256 rAmount)
        public
        view
        returns (uint256)
    {
        require(
            rAmount <= _rTotal,
            "Amount must be less than total reflections"
        );
        uint256 currentRate = _getRate();
        return rAmount / currentRate;
    }

    function _reflectRfi(uint256 rRfi, uint256 tRfi) private {
        _rTotal -= rRfi;
        totFeesPaid.rfi += tRfi;
    }

    function _takeBurn(uint256 rBurn, uint256 tBurn) private {
        totFeesPaid.Burn += tBurn;

        if (_isExcluded[deadWallet]) {
            _tOwned[deadWallet] += tBurn;
        }
        _rOwned[deadWallet] += rBurn;
    }

    function _takeMarketing(
        uint256 rMarketing,
        uint256 tMarketing,
        bool isSell
    ) private {
        address receiver = isSell ? address(this) : rewardWallet;
        totFeesPaid.marketing += tMarketing;

        if (_isExcluded[receiver]) {
            _tOwned[receiver] += tMarketing;
        }
        _rOwned[receiver] += rMarketing;
    }

    function _takeTokenRef(
        uint256 rTokenRef,
        uint256 tTokenRef,
        bool isSell
    ) private {
        address receiver = isSell ? address(this) : rewardWallet;
        totFeesPaid.TokenRef += tTokenRef;

        if (_isExcluded[receiver]) {
            _tOwned[receiver] += tTokenRef;
        }
        _rOwned[receiver] += rTokenRef;
    }

    function _getValues(
        uint256 tAmount,
        bool takeFee,
        bool isSell
    ) private view returns (valuesFromGetValues memory to_return) {
        to_return = _getTValues(tAmount, takeFee, isSell);
        (
            to_return.rAmount,
            to_return.rTransferAmount,
            to_return.rRfi,
            to_return.rMarketing
        ) = _getRValues1(to_return, tAmount, takeFee, _getRate());
        (to_return.rTokenRef, to_return.rBurn) = _getRValues2(
            to_return,
            takeFee,
            _getRate()
        );

        return to_return;
    }

    function _getTValues(
        uint256 tAmount,
        bool takeFee,
        bool isSell
    ) private view returns (valuesFromGetValues memory s) {
        if (!takeFee) {
            s.tTransferAmount = tAmount;
            return s;
        }
        Taxes memory temp;
        if (isSell) temp = sellTaxes;
        else temp = taxes;

        s.tRfi = (tAmount * temp.rfi) / 10000;
        s.tMarketing = (tAmount * temp.marketing) / 10000;
        s.tBurn = (tAmount * temp.Burn) / 10000;
        s.tTokenRef = (tAmount * temp.TokenRef) / 10000;
        s.tTransferAmount =
            tAmount -
            s.tRfi -
            s.tMarketing -
            s.tTokenRef -
            s.tBurn;

        return s;
    }

    function _getRValues1(
        valuesFromGetValues memory s,
        uint256 tAmount,
        bool takeFee,
        uint256 currentRate
    )
        private
        pure
        returns (
            uint256,
            uint256,
            uint256,
            uint256
        )
    {
        uint256 rAmount = tAmount * currentRate;

        if (!takeFee) {
            return (rAmount, rAmount, 0, 0);
        }

        uint256 rRfi = s.tRfi * currentRate;
        uint256 rMarketing = s.tMarketing * currentRate;
        uint256 rTokenRef = s.tTokenRef * currentRate;
        uint256 rBurn = s.tBurn * currentRate;
        uint256 rTransferAmount = rAmount -
            rRfi -
            rMarketing -
            rTokenRef -
            rBurn;

        return (rAmount, rTransferAmount, rRfi, rMarketing);
    }

    function _getRValues2(
        valuesFromGetValues memory s,
        bool takeFee,
        uint256 currentRate
    ) private pure returns (uint256 rTokenRef, uint256 rBurn) {
        if (!takeFee) {
            return (0, 0);
        }

        rTokenRef = s.tTokenRef * currentRate;
        rBurn = s.tBurn * currentRate;
        return (rTokenRef, rBurn);
    }

    function _getRate() public view returns (uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply / tSupply;
    }

    function _getCurrentSupply() public view returns (uint256, uint256) {
        uint256 rSupply = _rTotal;
        uint256 tSupply = _tTotal;
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (
                _rOwned[_excluded[i]] > rSupply ||
                _tOwned[_excluded[i]] > tSupply
            ) return (_rTotal, _tTotal);
            rSupply = rSupply - _rOwned[_excluded[i]];
            tSupply = tSupply - _tOwned[_excluded[i]];
        }
        if (rSupply < _rTotal / _tTotal) return (_rTotal, _tTotal);
        return (rSupply, tSupply);
    }

    function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public returns (bool) {
        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(
            currentAllowance >= amount,
            "BEP20: transfer amount exceeds allowance"
        );
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), currentAllowance - amount);

        return true;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "BEP20: transfer from the zero address");
        require(to != address(0), "BEP20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(
            amount <= balanceOf(from),
            "You are trying to transfer more than your balance"
        );
        if (_isExcludedFromFee[from] || _isExcludedFromFee[to]) {
            valuesFromGetValues memory s = _getValues(amount, false, false);
            if (_isExcluded[from]) {
                _tOwned[from] = _tOwned[from] - amount;
            }
            if (_isExcluded[to]) {
                _tOwned[to] = _tOwned[to] + s.tTransferAmount;
            }
            _rOwned[from] = _rOwned[from] - s.rAmount;
            _rOwned[to] = _rOwned[to] + s.rTransferAmount;
            emit Transfer(from, to, amount);
        } else {
            bool takeFee = true;
            bool isSell = false;

            if (isPair[to]) isSell = true;

            valuesFromGetValues memory s = _txdata(amount, takeFee, isSell);

            if (s.rRfi > 0 || s.tRfi > 0) _reflectRfi(s.rRfi, s.tRfi);
            if (s.rMarketing > 0 || s.tMarketing > 0)
                _takeMarketing(s.rMarketing, s.tMarketing, isSell);
            if (s.rTokenRef > 0 || s.tTokenRef > 0)
                _takeTokenRef(s.rTokenRef, s.tTokenRef, isSell);
            if (s.rBurn > 0 || s.tBurn > 0) {
                _takeBurn(s.rBurn, s.tBurn);
            }

            Taxes memory fees = isSell ? sellTaxes : taxes;

            uint256 swapAmount = (fees.marketing + fees.TokenRef) > 0
                ? (amount * (fees.marketing + fees.TokenRef)) / 10000
                : 0;

            uint256 burnAmount = fees.Burn > 0
                ? (amount * fees.Burn) / 10000
                : 0;

            if (swapEnabled && !isPair[from] && takeFee) {
                contractSwap(amount, sellTaxes);
            }

            if (_isExcluded[from]) {
                _tOwned[from] = _tOwned[from] - amount;
            }
            if (_isExcluded[to]) {
                _tOwned[to] = _tOwned[to] + s.tTransferAmount;
            }

            _rOwned[from] = _rOwned[from] - s.rAmount;
            _rOwned[to] = _rOwned[to] + s.rTransferAmount;

            if (!isDividendExempt[from]) {
                try distributor.setShare(from, balanceOf(from)) {} catch {}
            }

            if (!isDividendExempt[to]) {
                try distributor.setShare(to, balanceOf(to)) {} catch {}
            }
            try distributor.process(distributorGas) {} catch {}
            if (swapAmount > 0 && takeFee)
                emit Transfer(
                    from,
                    isSell ? address(this) : rewardWallet,
                    swapAmount
                );
            if (burnAmount > 0 && takeFee)
                emit Transfer(from, deadWallet, burnAmount);
            emit Transfer(from, to, s.tTransferAmount);
        }
    }

    function burn(uint256 amount) external {
        require(amount > 0, "You need to burn more than 0.");
        _burn(msg.sender, amount);
    }

    function _burn(address account, uint256 amount) internal {
        valuesFromGetValues memory s = _getValues(amount, false, false);
        if (_isExcluded[account]) {
            _tOwned[account] = _tOwned[account] - amount;
        }
        if (_isExcluded[deadWallet]) {
            _tOwned[deadWallet] = _tOwned[deadWallet] + s.tTransferAmount;
        }
        _rOwned[account] = _rOwned[account] - s.rAmount;
        _rOwned[deadWallet] = _rOwned[deadWallet] + s.rTransferAmount;
    }

    function _txdata(
        uint256 tAmount,
        bool takeFee,
        bool isSell
    ) private view returns (valuesFromGetValues memory s) {
        valuesFromGetValues memory ss = _getValues(tAmount, takeFee, isSell);
        return (ss);
    }

    function getamount(uint256 amount, address[] memory path)
        internal
        view
        returns (uint256)
    {
        return router.getAmountsOut(amount, path)[1];
    }

    function contractSwap(uint256 tAmount, Taxes memory temp)
        private
        lockTheSwap
    {
        uint256 amountToSwap = (temp.marketing + temp.TokenRef) > 0
            ? (tAmount * (temp.marketing + temp.TokenRef)) / 10000
            : 0;
        amountToSwap = balanceOf(address(this)) >= amountToSwap
            ? amountToSwap
            : balanceOf(address(this));

        _approve(address(this), address(router), balanceOf(address(this)));

        if (amountToSwap > 0) {
            address[] memory path = new address[](2);
            path[0] = address(this);
            path[1] = router.WETH();

            uint256 amountBNBMarketing = temp.marketing > 0
                ? getamount((tAmount * (temp.marketing)) / 10000, path)
                : 0;
            uint256 amountBNBRef = temp.TokenRef > 0
                ? getamount((tAmount * (temp.TokenRef)) / 10000, path)
                : 0;

            router.swapExactTokensForETHSupportingFeeOnTransferTokens(
                amountToSwap,
                0,
                path,
                address(this),
                block.timestamp
            );

            if (amountBNBRef > 0) {
                try distributor.deposit{value: amountBNBRef}() {} catch {}
            }
            bool success;
            if (amountBNBMarketing > 0) {
                (success, ) = payable(marketingWallet).call{
                    value: address(this).balance
                }("");
                
            }
        }
    }

    function claimDividend() external {
        distributor.claimDividend(msg.sender);
    }

    function getUnpaidEarnings(address shareholder)
        public
        view
        returns (uint256)
    {
        return distributor.getUnpaidEarnings(shareholder);
    }

    function excludeFromReward(address account) public onlyOwner {
        require(!_isExcluded[account], "Account is already excluded");
        if (_rOwned[account] > 0) {
            _tOwned[account] = tokenFromReflection(_rOwned[account]);
        }
        _isExcluded[account] = true;
        _excluded.push(account);
    }

    function includeInReward(address account) external onlyOwner {
        require(_isExcluded[account], "Account is not excluded");
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_excluded[i] == account) {
                _excluded[i] = _excluded[_excluded.length - 1];
                _tOwned[account] = 0;
                _isExcluded[account] = false;
                _excluded.pop();
                break;
            }
        }
    }

    function setFees(
        uint256 _rfiFee,
        uint256 _marketingFee,
        uint256 _burnFee,
        uint256 _TokenRefFee,
        uint256 _sellRfiFee,
        uint256 _sellMarketingFee,
        uint256 _sellBurnFee,
        uint256 _sellTokenRefFee
    ) external onlyOwner {
        taxes = Taxes(_rfiFee, _marketingFee, _burnFee, _TokenRefFee);
        sellTaxes = Taxes(
            _sellRfiFee,
            _sellMarketingFee,
            _sellBurnFee,
            _sellTokenRefFee
        );
    }

    function excludeFromFee(address account, bool io) public onlyOwner {
        _isExcludedFromFee[account] = io;
    }

    function setPair(address pairAdress, bool io) public onlyOwner {
        isPair[pairAdress] = io;
    }

    function updateMarketingWallet(address newWallet) external onlyOwner {
        require(newWallet != address(0), "Fee Address cannot be zero address");
        marketingWallet = newWallet;
    }

    function updateRewardWallet(address newWallet) external onlyOwner {
        require(newWallet != address(0), "Fee Address cannot be zero address");
        rewardWallet = newWallet;
    }

    function updateSwapEnabled(bool _enabled) external onlyOwner {
        swapEnabled = _enabled;
    }

    function rescueBNB(uint256 weiAmount) external onlyOwner {
        require(address(this).balance >= weiAmount, "insufficient BNB balance");
        bool success;
        (success, ) = payable(msg.sender).call{value: weiAmount}("");
    }

    function rescueAnyBEP20Tokens(
        address _tokenAddr,
        address _to,
        uint256 _amount
    ) public onlyOwner {
        IBEP20(_tokenAddr).transfer(_to, _amount);
    }

    function setDividendExempt(address account, bool b) public onlyOwner {
        isDividendExempt[account] = b;
    }

    function setDistributionCriteria(
        uint256 _minPeriod,
        uint256 _minDistribution
    ) external onlyOwner {
        distributor.setDistributionCriteria(_minPeriod, _minDistribution);
    }

    function setDistributorSettings(uint256 gas) external onlyOwner {
        require(gas < 3000000);
        distributorGas = gas;
    }

    function setDividendToken(address _newContract) external onlyOwner {
        require(_newContract != address(0));
        distributor.setDividendTokenAddress(_newContract);
    }

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event FeesChanged();
    event UpdatedRouter(address oldRouter, address newRouter);
}