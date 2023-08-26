// SPDX-License-Identifier: UNLICENSE

/*
PEPE Rewards + PERA Reflections + Buyback&Burn = Infinite Money Glitch! 

Telegram - https://t.me/thepeperazzi

*/

pragma solidity 0.8.19;

interface IBEP20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

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
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IRouter {
     function factory() external pure returns (address);
    function WETH() external pure returns (address);

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

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    
    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

library Address {
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
}


library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     *
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     *
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts with custom message when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

interface IDividendDistributor {
    function setDistributionCriteria(uint256 _minPeriod, uint256 _minDistribution) external;
    function setShare(address shareholder, uint256 amount) external;
    function deposit() external payable;
    function process(uint256 gas) external;
}

contract DividendDistributor is IDividendDistributor {
    using SafeMath for uint256;

    address _token;

    struct Share {
        uint256 amount;
        uint256 totalExcluded;
        uint256 totalRealised;
    }

    IBEP20 BEP_TOKEN;

    address WBNB;
    IRouter router;

    address[] shareholders;
    mapping(address => uint256) shareholderIndexes;
    mapping(address => uint256) shareholderClaims;

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
    modifier initialization() {
        require(!initialized);
        _;
        initialized = true;
    }

    modifier onlyFactory() {
        require(msg.sender == _token);
        _;
    }

    constructor(
        address _router,
        address _BEP_TOKEN,
        address _wbnb
    ) {
        router = _router != address(0)
            ? IRouter(_router)
            : IRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _token = msg.sender;
        BEP_TOKEN = IBEP20(_BEP_TOKEN);
        WBNB = _wbnb;
    }

    function setDistributionCriteria(
        uint256 _minPeriod,
        uint256 _minDistribution
    ) external override onlyFactory {
        minPeriod = _minPeriod;
        minDistribution = _minDistribution;
    }

    function setShare(address shareholder, uint256 amount)
        external
        override
        onlyFactory
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

    function deposit() external payable override onlyFactory {
        uint256 balanceBefore = BEP_TOKEN.balanceOf(address(this));

        address[] memory path = new address[](2);
        path[0] = WBNB;
        path[1] = address(BEP_TOKEN);
        router.swapExactETHForTokensSupportingFeeOnTransferTokens{
            value: msg.value
        }(0, path, address(this), block.timestamp);

        uint256 amount = BEP_TOKEN.balanceOf(address(this)).sub(balanceBefore);

        totalDividends = totalDividends.add(amount);
        dividendsPerShare = dividendsPerShare.add(
            dividendsPerShareAccuracyFactor.mul(amount).div(totalShares)
        );
    }

    function process(uint256 gas) external override onlyFactory {
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
            BEP_TOKEN.transfer(shareholder, amount);
            shareholderClaims[shareholder] = block.timestamp;
            shares[shareholder].totalRealised = shares[shareholder]
                .totalRealised
                .add(amount);
            shares[shareholder].totalExcluded = getCumulativeDividends(
                shares[shareholder].amount
            );
        }
    }

    function claimDividend() external {
        distributeDividend(msg.sender);
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
        internal
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

    function getShareholders()
        external
        view
        onlyFactory
        returns (address[] memory)
    {
        return shareholders;
    }

    function getShareholderAmount(address shareholder)
        external
        view
        returns (uint256)
    {
        return shares[shareholder].amount;
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
}

contract PEPERAZZI is Context, IBEP20, Ownable {
    using Address for address payable;
    using SafeMath for uint256;

    uint256 public maxTransactionAmount =  1666666  * (10**18); //1.5%

    // to hold last Transfers temporarily during launch
    mapping(address => uint256) private _holderLastTransferTimestamp; 
    bool public transferDelayEnabled = true;

    mapping(address => uint256) private _rOwned;
    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;
    mapping(address => bool) private _isExcluded;
    mapping (address => bool) isDividendExempt;

    mapping (address => bool) public pairs;

    address[] private _excluded;

    bool private swapping;
    bool public tradingIsEnabled = false;
    
    IRouter public router;
    address public pair;

    DividendDistributor public distributor;
    // use by default 300,000 gas to process auto-claiming dividends
    uint256 distributorGas = 300000;

    uint8 private constant _decimals = 18;
    uint256 private constant MAX = ~uint256(0);

    uint256 private _tTotal = 111111111 * 10**_decimals;
    uint256 private _rTotal = (MAX - (MAX % _tTotal));

    uint256 public swapTokensAtAmount = 100_000 * 10**_decimals;

    event SwapAndLiquify(
        uint256 tokensSwapped,
        uint256 ethReceived,
        uint256 tokensIntoLiqudity,
        bool success
    );

    address PEPE = 0x25d887Ce7a35172C62FeBFD67a1856F20FaEbB00; // PEPE

    address public deadWallet = 0x000000000000000000000000000000000000dEaD;
    address public marketingWallet = 0x76fE4E8aD5aE03A29e8049C270aD0ACd828fc1e6;
    address public liquidityWallet = 0xa7b8356dbCD68523A38B29C22b02B633d8c0541F;

    string private constant _name = "PEPERAZZI";
    string private constant _symbol = "PERA";

    struct BuyTaxes {
        uint256 rfi;
        uint256 marketing;
        uint256 lp;
        uint256 rewardToken;
    }

    struct SellTaxes {
        uint256 rfi;
        uint256 marketing;
        uint256 lp;
        uint256 rewardToken;
    }

    // tax reflection, mkt, lp, PEPE Rewards
    BuyTaxes public taxes = BuyTaxes(2, 2, 1, 5);
    uint256 public buyTotalFees = taxes.rfi.add(taxes.marketing).add(taxes.lp).add(taxes.rewardToken);
    SellTaxes public selltaxes = SellTaxes(2, 2, 1, 5);
    uint256 public sellTotalFees = selltaxes.rfi.add(selltaxes.marketing).add(selltaxes.lp).add(selltaxes.rewardToken);

    struct TotFeesPaidStruct {
        uint256 rfi;
        uint256 marketing;
        uint256 lp;
        uint256 rewardToken;
    }

    TotFeesPaidStruct public totFeesPaid;

    struct valuesFromGetValues {
        uint256 rAmount;
        uint256 rTransferAmount;
        uint256 rRfi;
        uint256 rMarketing;
        uint256 rRewardToken;
        uint256 rLP;
        uint256 tTransferAmount;
        uint256 tRfi;
        uint256 tMarketing;
        uint256 tRewardToken;
        uint256 tLP;
    }

    modifier lockTheSwap() {
        swapping = true;
        _;
        swapping = false;
    }

    constructor() {
       IRouter _router = IRouter(address(0x10ED43C718714eb63d5aA57B78B54704E256024E));
        address _pair = IFactory(_router.factory()).createPair(address(this), _router.WETH());

        router = _router;
        pair = _pair;
        pairs[pair] = true;

        distributor = new DividendDistributor(address(router), PEPE, _router.WETH());

        excludeFromReward(address(this));
        excludeFromReward(pair);
        excludeFromReward(deadWallet);

        isDividendExempt[pair] = true;
        isDividendExempt[address(this)] = true;
        isDividendExempt[deadWallet] = true;

        _rOwned[owner()] = _rTotal;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[marketingWallet] = true;
        _isExcludedFromFee[deadWallet] = true;
        emit Transfer(address(0), owner(), _tTotal);
    }

    //std BEP20:
    function name() public pure returns (string memory) {
        return _name;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    //override BEP20:
    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function getrewardDistributionTime()external view returns(uint256){return distributor.minPeriod();}
    function getRewardDistributionMinAmount() external view returns(uint256){return distributor.minDistribution();}

    function balanceOf(address account) public view override returns (uint256) {
        if (_isExcluded[account]) return _tOwned[account];
        return tokenFromReflection(_rOwned[account]);
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "BEP20: transfer amount exceeds allowance");
        _approve(sender, _msgSender(), currentAllowance - amount);

        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        returns (bool)
    {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(currentAllowance >= subtractedValue, "BEP20: decreased allowance below zero");
        _approve(_msgSender(), spender, currentAllowance - subtractedValue);

        return true;
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function isExcludedFromReward(address account) public view returns (bool) {
        return _isExcluded[account];
    }


    function reflectionFromToken(uint256 tAmount, bool deductTransferRfi)
        public
        view
        returns (uint256)
    {
        require(tAmount <= _tTotal, "Amount must be less than supply");
        if (!deductTransferRfi) {
            valuesFromGetValues memory s = _getValues(tAmount, true);
            return s.rAmount;
        } else {
            valuesFromGetValues memory s = _getValues(tAmount, true);
            return s.rTransferAmount;
        }
    }

    // remove transfer delay after launch
    function disableTransferDelay() external onlyOwner {
        transferDelayEnabled = false;
    }

    function setDistributorGas(uint256 newValue) external onlyOwner {
        require(newValue >= 200000 && newValue <= 500000, "PERA: gasForProcessing must be between 200,000 and 500,000");
        require(newValue != distributorGas, "PERA: Cannot update gasForProcessing to same value");
        distributorGas = newValue;
    }

    function updateMaxTransactionAmount(uint256 amount) external onlyOwner {
         require(amount > 1111111, "Max Transaction Amount must be greater than 1% of total supply");
        maxTransactionAmount = amount * 10**_decimals;
    }

    function tokenFromReflection(uint256 rAmount) public view returns (uint256) {
        require(rAmount <= _rTotal, "Amount must be less than total reflections");
        uint256 currentRate = _getRate();
        return rAmount / currentRate;
    }

    //@dev kept original RFI naming -> "reward" as in reflection
    function excludeFromReward(address account) public onlyOwner {
        require(!_isExcluded[account], "Account is already excluded");
        if (_rOwned[account] > 0) {
            _tOwned[account] = tokenFromReflection(_rOwned[account]);
        }
        _isExcluded[account] = true;
        _excluded.push(account);
    }

    function setDistributor(DividendDistributor dist)external onlyOwner {
        distributor = dist;
    }

    function setTradingIsEnabled() external onlyOwner {
        tradingIsEnabled = true;
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

    function setIsDividendExempt(address holder, bool exempt) external onlyOwner {
        require(holder != address(this) && !pairs[holder]);
        isDividendExempt[holder] = exempt;
        if(exempt){
            distributor.setShare(holder, 0);
        }else{

            distributor.setShare(holder, balanceOf(holder));

            
        }
    }

    function excludeFromFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = true;
    }

    function includeInFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = false;
    }

    function isExcludedFromFee(address account) public view returns (bool) {
        return _isExcludedFromFee[account];
    }

    function _reflectRfi(uint256 rRfi, uint256 tRfi) private {
        _rTotal -= rRfi;
        totFeesPaid.rfi += tRfi;
    }


    function _takeMarketing(uint256 rMarketing, uint256 tMarketing) private {
        totFeesPaid.marketing += tMarketing;

        if (_isExcluded[address(this)]) {
            _tOwned[address(this)] += tMarketing;
        }
        _rOwned[address(this)] += rMarketing;
    }

    function _takeRewardToken(uint256 rRewardToken, uint256 tRewardToken) private {
        totFeesPaid.rewardToken += tRewardToken;

        if (_isExcluded[address(this)]) {
            _tOwned[address(this)] += tRewardToken;
        }
        _rOwned[address(this)] += rRewardToken;
    }

    function _takeLP(uint256 rLP, uint256 tLP) private {
        totFeesPaid.lp += tLP;

        if (_isExcluded[address(this)]) {
            _tOwned[address(this)] += tLP;
        }
        _rOwned[address(this)] += rLP;
    }

    function _getValues(
        uint256 tAmount,
        bool takeFee
    ) private view returns (valuesFromGetValues memory to_return) {
        to_return = _getTValues(tAmount, takeFee);
        (
            to_return.rAmount,
            to_return.rTransferAmount,
            to_return.rRfi,
            to_return.rMarketing,
            to_return.rRewardToken,
            to_return.rLP
        ) = _getRValues(to_return, tAmount, takeFee, _getRate());

        return to_return;
    }

    function _getTValues(
        uint256 tAmount,
        bool takeFee
    ) private view returns (valuesFromGetValues memory s) {
        if (!takeFee) {
            s.tTransferAmount = tAmount;
            return s;
        }

        s.tRfi = (tAmount * taxes.rfi) / 100;
        s.tMarketing = (tAmount * taxes.marketing) / 100;
        s.tRewardToken = (tAmount * taxes.rewardToken) / 100;
        s.tLP = (tAmount * taxes.lp) / 100;
        s.tTransferAmount =
            tAmount -
            s.tRfi -
            s.tMarketing -
            s.tRewardToken -
            s.tLP;
        return s;
    }

    function _getRValues(
        valuesFromGetValues memory s,
        uint256 tAmount,
        bool takeFee,
        uint256 currentRate
    )
        private
        pure
        returns (
            uint256 rAmount,
            uint256 rTransferAmount,
            uint256 rRfi,
            uint256 rMarketing,
            uint256 rRewardToken,
            uint256 rLP
        )
    {
        rAmount = tAmount * currentRate;

        if (!takeFee) {
            return (rAmount, rAmount, 0, 0, 0, 0);
        }

        rRfi = s.tRfi * currentRate;
        rMarketing = s.tMarketing * currentRate;
        rRewardToken = s.tRewardToken * currentRate;
        rLP = s.tLP * currentRate;
        rTransferAmount =
            rAmount -
            rRfi -
            rMarketing -
            rRewardToken - 
            rLP;
        return (rAmount, rTransferAmount, rRfi, rMarketing, rRewardToken, rLP);
    }

    function _getRate() private view returns (uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply / tSupply;
    }

    function _getCurrentSupply() private view returns (uint256, uint256) {
        uint256 rSupply = _rTotal;
        uint256 tSupply = _tTotal;
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_rOwned[_excluded[i]] > rSupply || _tOwned[_excluded[i]] > tSupply)
                return (_rTotal, _tTotal);
            rSupply = rSupply - _rOwned[_excluded[i]];
            tSupply = tSupply - _tOwned[_excluded[i]];
        }
        if (rSupply < _rTotal / _tTotal) return (_rTotal, _tTotal);
        return (rSupply, tSupply);
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

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "BEP20: transfer from the zero address");
        require(to != address(0), "BEP20: transfer to the zero address");
        require(tradingIsEnabled || (_isExcludedFromFee[from] || _isExcludedFromFee[to]), "Trading not started");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(
            amount <= balanceOf(from),
            "You are trying to transfer more than your balance"
        );

          // Prevent buying more than 1 txn each 5 seconds at launch. Bot killer. Will be removed shortly after launch.
        if (transferDelayEnabled){
            if (pairs[from] && !_isExcludedFromFee[to]){
                require(_holderLastTransferTimestamp[to] < block.timestamp, "_transfer:: Transfer Delay enabled.  Please try again later.");
                _holderLastTransferTimestamp[to] = block.timestamp + 5;
            }
        }


        bool isSelling = pairs[to];
        bool isBuying =  pairs[from];

        uint256 contractTokenBalance = balanceOf(address(this));
        bool canSwap = contractTokenBalance >= swapTokensAtAmount;

            uint256 oldBuyRFI = taxes.rfi;
            uint256 oldBuyMarketing = taxes.marketing;
            uint256 oldBuyRewardToken = taxes.rewardToken;
            uint256 oldBuyLP = taxes.lp;

        if(isSelling){
        
            taxes.rfi = selltaxes.rfi;
            taxes.marketing = selltaxes.marketing;
            taxes.rewardToken = selltaxes.rewardToken;
            taxes.lp = selltaxes.lp;
        }

        if (
            isBuying &&
             !_isExcludedFromFee[from] &&
             !_isExcludedFromFee[to]
        ) {
            
            require(amount <= maxTransactionAmount, "maximum transaction amount reached.");
        }

        if ( 
            isSelling &&
             !_isExcludedFromFee[from] &&
             !_isExcludedFromFee[to]
        ) {

           require(amount <= maxTransactionAmount, "maximum transaction amount reached.");
        }

        if (
            !swapping &&
            canSwap &&
            isSelling &&
            !_isExcludedFromFee[from] &&
            !_isExcludedFromFee[to]
        ) {
            swapAndLiquify(contractTokenBalance);
        }
        bool takeFee = true;
        if (swapping || _isExcludedFromFee[from] || _isExcludedFromFee[to]) takeFee = false;

        if (!isSelling && !isBuying) takeFee = false;

        _tokenTransfer(from, to, amount, takeFee);
        
        if(!isDividendExempt[from]){ try distributor.setShare(from, balanceOf(from)) {} catch {} }        
        if(!isDividendExempt[to]){ try distributor.setShare(to, balanceOf(to)) {} catch {} }

        if(isSelling){
           
            taxes.rfi = oldBuyRFI;
            taxes.marketing = oldBuyMarketing;
            taxes.rewardToken = oldBuyRewardToken;
            taxes.lp = oldBuyLP;

        }

        try distributor.process(distributorGas) {} catch {}
    }

    //this method is responsible for taking all fee, if takeFee is true
    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {

        valuesFromGetValues memory s = _getValues(tAmount, takeFee);

        if (_isExcluded[sender]) {
            //from excluded
            _tOwned[sender] = _tOwned[sender] - tAmount;
        }
        if (_isExcluded[recipient]) {
            //to excluded
            _tOwned[recipient] = _tOwned[recipient] + s.tTransferAmount;
        }

        _rOwned[sender] = _rOwned[sender] - s.rAmount;
        _rOwned[recipient] = _rOwned[recipient] + s.rTransferAmount;

        if (s.rRfi > 0 || s.tRfi > 0) _reflectRfi(s.rRfi, s.tRfi);
        if (s.rMarketing > 0 || s.tMarketing > 0) _takeMarketing(s.rMarketing, s.tMarketing);
        if (s.rRewardToken > 0 || s.tRewardToken > 0) _takeRewardToken(s.rRewardToken, s.tRewardToken);
        if (s.rLP > 0 || s.tLP > 0) _takeLP(s.rLP, s.tLP);
        emit Transfer(sender, recipient, s.tTransferAmount);
    }

    function swapAndLiquify(uint256 tokens) private lockTheSwap {
       // Split the contract balance into halves
        uint256 denominator = sellTotalFees.sub(selltaxes.rfi).mul(2);
        uint256 tokensToAddLiquidityWith = tokens.mul(selltaxes.lp).div(denominator);
        uint256 toSwap = tokens.sub(tokensToAddLiquidityWith);

        uint256 initialBalance = address(this).balance;

        swapTokensForBNB(toSwap);

        uint256 deltaBalance = address(this).balance.sub(initialBalance);
        uint256 unitBalance= deltaBalance.div(denominator).sub(selltaxes.lp);
        uint256 bnbToAddLiquidityWith = unitBalance.mul(selltaxes.lp);

        if(bnbToAddLiquidityWith > 0){
            // Add liquidity to pancakeswap
            addLiquidity(tokensToAddLiquidityWith, bnbToAddLiquidityWith);
            emit SwapAndLiquify(toSwap, bnbToAddLiquidityWith, tokensToAddLiquidityWith, true);
        }

        uint256 dividends = unitBalance * 2 * selltaxes.rewardToken;
        uint256 balanceBefore = address(this).balance;
        swapTokensForBNB(dividends);

        uint256 amountBNB = address(this).balance.sub(balanceBefore);
         if(amountBNB > 0){
            try distributor.deposit{value: amountBNB}() {} catch {}
        }
      
        uint256 marketingAmt = unitBalance.mul(2).mul(selltaxes.marketing);
        if(marketingAmt > 0){
            payable(marketingWallet).transfer(address(this).balance);
        }

    }
   
    function addLiquidity(uint256 tokenAmount, uint256 ethAmount) private {
        // approve token transfer to cover all possible scenarios
        _approve(address(this), address(router), tokenAmount);

        // add the liquidity
        router.addLiquidityETH{value: ethAmount}(
            address(this),
            tokenAmount,
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            liquidityWallet,
            block.timestamp
        );
    }



    function swapTokensForBNB(uint256 tokenAmount) private {
        // generate the pair path of token -> weth
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();

        _approve(address(this), address(router), tokenAmount);

        // make the swap
        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of ETH
            path,
            address(this),
            block.timestamp
        );
    }

    function bulkExcludeFee(address[] memory accounts, bool state) external onlyOwner {
        for (uint256 i = 0; i < accounts.length; i++) {
            _isExcludedFromFee[accounts[i]] = state;
        }
    }

    function updateMarketingWallet(address newWallet) external onlyOwner {
        require(newWallet != address(0),"Fee Address cannot be zero address");
        marketingWallet = newWallet;
    }

    function updateSwapTokensAtAmount(uint256 amount) external onlyOwner {
        require(amount > totalSupply() / 1000000, "SwapTokensAtAmount must be greater than 0.0001% of total supply");
        swapTokensAtAmount = amount * 10**_decimals;
    }

    function updateBuyTax(uint256 _rfl, uint256 _marketing, uint256 _rewardToken, uint256 _lp) external onlyOwner {
        uint256 _totalBuyFees = _rfl.add(_marketing).add(_rewardToken).add(_lp);

        require(_totalBuyFees <= 10, "Cannot be total fees higher than 10%");
        taxes.rfi = _rfl;
        taxes.marketing = _marketing;
        taxes.rewardToken = _rewardToken;
        taxes.lp = _lp;
    }

    function updateSellTax(uint256 _rfl, uint256 _marketing, uint256 _rewardToken, uint256 _lp) external onlyOwner {
        uint256 _totalSellFees = _rfl.add(_marketing).add(_rewardToken).add(_lp);

        require(_totalSellFees <= 10, "Cannot be total fees higher than 10%");
        selltaxes.rfi = _rfl;
        selltaxes.marketing = _marketing;
        selltaxes.rewardToken = _rewardToken;
        selltaxes.lp = _lp;
    }

    receive() external payable {}
}