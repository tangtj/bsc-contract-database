// SPDX-License-Identifier: Unlicensed
pragma solidity ^0.8.4;

abstract contract Context {

    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this;
        // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

interface IERC20 {

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

library SafeMath {

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

library Address {

    function isContract(address account) internal view returns (bool) {
        // According to EIP-1052, 0x0 is the value returned for not-yet created accounts
        // and 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470 is returned
        // for accounts without code, i.e. `keccak256('')`
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly {codehash := extcodehash(account)}
        return (codehash != accountHash && codehash != 0x0);
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success,) = recipient.call{value : amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCall(target, data, "Address: low-level call failed");
    }

    function functionCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
        return _functionCallWithValue(target, data, 0, errorMessage);
    }

    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    function functionCallWithValue(address target, bytes memory data, uint256 value, string memory errorMessage) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        return _functionCallWithValue(target, data, value, errorMessage);
    }

    function _functionCallWithValue(address target, bytes memory data, uint256 weiValue, string memory errorMessage) private returns (bytes memory) {
        require(isContract(target), "Address: call to non-contract");

        (bool success, bytes memory returndata) = target.call{value : weiValue}(data);
        if (success) {
            return returndata;
        } else {

            if (returndata.length > 0) {
                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
    }
}

contract Ownable is Context {
    address public _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);


    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function waiveOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }

    function getTime() public view returns (uint256) {
        return block.timestamp;
    }

}

interface IUniswapV2Factory {

    function getPair(address tokenA, address tokenB) external view returns (address pair);

    function createPair(address tokenA, address tokenB) external returns (address pair);

}

interface IUniswapV2Router01 {
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

    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);

    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);


}

interface IUniswapV2Router02 is IUniswapV2Router01 {

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

contract Token is Context, IERC20, Ownable {

    using SafeMath for uint256;
    using Address for address;

    string private _name;
    string private _symbol;
    uint8 private _decimals;
    address payable public marketingWalletAddress;
    address payable public teamWalletAddress;

    address payable public fundraisingAddr;
    address payable public developerAddr;
    address payable public projectTeamAddr;
    address payable public airdropAddr;
    address payable public foundationAddr;
    address payable public stakingIncomeAddr;
    address payable public entityX2EAddr;
    address payable public financialTeamAddr;

    address public deadAddress = 0x000000000000000000000000000000000000dEaD;

    mapping(address => uint256) _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) public _isBlacklisted;
    mapping(address => bool) public isExcludedFromFee;
    mapping(address => bool) public isWalletLimitExempt;
    mapping(address => bool) public isTxLimitExempt;
    mapping(address => bool) public isMarketPair;

    uint256 public _buyLiquidityFee = 2;
    uint256 public _buyMarketingFee = 3;
    uint256 public _buyTeamFee = 4;
    uint256 public _buyDestroyFee = 0;

    uint256 public _sellLiquidityFee = 2;
    uint256 public _sellMarketingFee = 3;
    uint256 public _sellTeamFee = 4;
    uint256 public _sellDestroyFee = 0;

    uint256 public _liquidityShare = 1;
    uint256 public _marketingShare = 3;
    uint256 public _teamShare = 3;
    uint256 public _totalDistributionShares = 7;

    uint256 public _totalTaxIfBuying = 7;
    uint256 public _totalTaxIfSelling = 7;

    uint256 public _tFeeTotal;
    uint256 public _maxDestroyAmount;
    uint256 private _totalSupply;
    uint256 public _maxTxAmount;
    uint256 public _walletMax;
    uint256 private _minimumTokensBeforeSwap = 0;
    uint256 public airdropNumbs;
    address private receiveAddress;
    uint256 public first;
    uint256 public kill = 0;

    address public usdt;
    address public ldao;

    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapPair;
    address public uniswapPairBNB;

    bool inSwapAndLiquify;
    bool public swapAndLiquifyEnabled = true;
    bool public swapAndLiquifyByLimitOnly = false;
    bool public checkWalletLimit = true;

    //LP Dividend
    mapping(address => bool) public isDividendExempt;
    mapping(address => bool) public _updated;
    uint256 public minPeriod = 5 minutes;
    uint256 public dividendTime;
    uint256 distributorGas = 500000;
    address[] public shareholders;
    uint256 currentIndex;
    mapping(address => uint256) public shareholderIndexes;
    uint256 public minDistribution = 0;

    //Open market
    bool public openMarket = false;

    event SwapAndLiquifyEnabledUpdated(bool enabled);

    event SwapTokensForETH(
        uint256 amountIn,
        address[] path
    );

    event FAILED_swapExactTokensForTokensSupportingFeeOnTransferTokens();

    modifier lockTheSwap {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }

    constructor (
        string memory coinName,
        string memory coinSymbol,
        uint8 coinDecimals,
        uint256 supply,
        address router,
        address owner,
        address marketingAddress,
        address teamAddress,
        address[] memory projectAddress,
        address usd,
        address _ldao
    ) payable {
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(router);
        uniswapPair = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), usd);
        uniswapPairBNB = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), _uniswapV2Router.WETH());

        usdt = usd;
        ldao = _ldao;
        _name = coinName;
        _symbol = coinSymbol;
        _decimals = coinDecimals;
        _owner = owner;
        receiveAddress = owner;
        _totalSupply = supply * 10 ** _decimals;
        _maxTxAmount = supply * 10 ** _decimals;
        _walletMax = supply * 10 ** _decimals;
        _maxDestroyAmount = supply * 10 ** _decimals;
        _minimumTokensBeforeSwap = 1 * 10 ** _decimals;
        marketingWalletAddress = payable(marketingAddress);
        teamWalletAddress = payable(teamAddress);

        fundraisingAddr = payable(projectAddress[0]);
        developerAddr = payable(projectAddress[1]);
        projectTeamAddr = payable(projectAddress[2]);
        airdropAddr = payable(projectAddress[3]);
        foundationAddr = payable(projectAddress[4]);
        stakingIncomeAddr = payable(projectAddress[5]);
        entityX2EAddr = payable(projectAddress[6]);
        financialTeamAddr = payable(projectAddress[7]);

        _totalTaxIfBuying = _buyLiquidityFee.add(_buyMarketingFee).add(_buyTeamFee);
        _totalTaxIfSelling = _sellLiquidityFee.add(_sellMarketingFee).add(_sellTeamFee);
        _totalDistributionShares = _liquidityShare.add(_marketingShare).add(_teamShare);
        uniswapV2Router = _uniswapV2Router;
        _allowances[address(this)][address(uniswapV2Router)] = _totalSupply;

        _balances[fundraisingAddr] += _totalSupply.mul(10).div(100);
        _balances[developerAddr] += _totalSupply.mul(5).div(100);
        _balances[projectTeamAddr] += _totalSupply.mul(1725).div(10000);
        _balances[airdropAddr] += _totalSupply.mul(75).div(10000);
        _balances[foundationAddr] += _totalSupply.mul(15).div(100);
        _balances[stakingIncomeAddr] += _totalSupply.mul(30).div(100);
        _balances[entityX2EAddr] += _totalSupply.mul(20).div(100);
        _balances[financialTeamAddr] += _totalSupply.mul(2).div(100);

        isExcludedFromFee[owner] = true;
        isExcludedFromFee[address(this)] = true;
        isExcludedFromFee[fundraisingAddr] = true;
        isExcludedFromFee[developerAddr] = true;
        isExcludedFromFee[projectTeamAddr] = true;
        isExcludedFromFee[airdropAddr] = true;
        isExcludedFromFee[foundationAddr] = true;
        isExcludedFromFee[stakingIncomeAddr] = true;
        isExcludedFromFee[entityX2EAddr] = true;
        isExcludedFromFee[financialTeamAddr] = true;

        isWalletLimitExempt[owner] = true;
        isWalletLimitExempt[address(uniswapPair)] = true;
        isWalletLimitExempt[address(uniswapPairBNB)] = true;
        isWalletLimitExempt[address(this)] = true;
        isWalletLimitExempt[deadAddress] = true;
        isWalletLimitExempt[fundraisingAddr] = true;
        isWalletLimitExempt[developerAddr] = true;
        isWalletLimitExempt[projectTeamAddr] = true;
        isWalletLimitExempt[airdropAddr] = true;
        isWalletLimitExempt[foundationAddr] = true;
        isWalletLimitExempt[stakingIncomeAddr] = true;
        isWalletLimitExempt[entityX2EAddr] = true;
        isWalletLimitExempt[financialTeamAddr] = true;

        isTxLimitExempt[owner] = true;
        isTxLimitExempt[deadAddress] = true;
        isTxLimitExempt[address(this)] = true;
        isTxLimitExempt[fundraisingAddr] = true;
        isTxLimitExempt[developerAddr] = true;
        isTxLimitExempt[projectTeamAddr] = true;
        isTxLimitExempt[airdropAddr] = true;
        isTxLimitExempt[foundationAddr] = true;
        isTxLimitExempt[stakingIncomeAddr] = true;
        isTxLimitExempt[entityX2EAddr] = true;
        isTxLimitExempt[financialTeamAddr] = true;

        //LP Dividend
        isDividendExempt[owner] = true;
        isDividendExempt[deadAddress] = true;
        isDividendExempt[address(this)] = true;
        isDividendExempt[fundraisingAddr] = true;
        isDividendExempt[developerAddr] = true;
        isDividendExempt[projectTeamAddr] = true;
        isDividendExempt[airdropAddr] = true;
        isDividendExempt[foundationAddr] = true;
        isDividendExempt[stakingIncomeAddr] = true;
        isDividendExempt[entityX2EAddr] = true;
        isDividendExempt[financialTeamAddr] = true;

        isMarketPair[address(uniswapPair)] = true;
        isMarketPair[address(uniswapPairBNB)] = true;

    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }

    function minimumTokensBeforeSwapAmount() public view returns (uint256) {
        return _minimumTokensBeforeSwap;
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function multipleBotlistAddress(address[] calldata accounts, bool excluded) public onlyOwner {
        for (uint256 i = 0; i < accounts.length; i++) {
            _isBlacklisted[accounts[i]] = excluded;
        }
    }

    function setMarketPairStatus(address account, bool newValue) public onlyOwner {
        isMarketPair[account] = newValue;
    }

    function setIsTxLimitExempt(address holder, bool exempt) external onlyOwner {
        isTxLimitExempt[holder] = exempt;
    }

    function setIsExcludedFromFee(address account, bool newValue) public onlyOwner {
        isExcludedFromFee[account] = newValue;
    }

    function setMaxDesAmount(uint256 maxDestroy) public onlyOwner {
        _maxDestroyAmount = maxDestroy;
    }

    function setBuyDestFee(uint256 newBuyDestroyFee) public onlyOwner {
        _buyDestroyFee = newBuyDestroyFee;
        _totalTaxIfBuying = _buyLiquidityFee.add(_buyMarketingFee).add(_buyTeamFee).add(_buyDestroyFee);
    }

    function setSellDestFee(uint256 newSellDestroyFee) public onlyOwner {
        _sellDestroyFee = newSellDestroyFee;
        _totalTaxIfSelling = _sellLiquidityFee.add(_sellMarketingFee).add(_sellTeamFee).add(_sellDestroyFee);
    }

    function setBuyTaxes(uint256 newLiquidityTax, uint256 newMarketingTax, uint256 newTeamTax) external onlyOwner() {
        _buyLiquidityFee = newLiquidityTax;
        _buyMarketingFee = newMarketingTax;
        _buyTeamFee = newTeamTax;

        _totalTaxIfBuying = _buyLiquidityFee.add(_buyMarketingFee).add(_buyTeamFee).add(_buyDestroyFee);
    }

    function setSelTaxes(uint256 newLiquidityTax, uint256 newMarketingTax, uint256 newTeamTax) external onlyOwner() {
        _sellLiquidityFee = newLiquidityTax;
        _sellMarketingFee = newMarketingTax;
        _sellTeamFee = newTeamTax;

        _totalTaxIfSelling = _sellLiquidityFee.add(_sellMarketingFee).add(_sellTeamFee).add(_sellDestroyFee);
    }

    function setDistributionSettings(uint256 newLiquidityShare, uint256 newMarketingShare, uint256 newTeamShare) external onlyOwner() {
        _liquidityShare = newLiquidityShare;
        _marketingShare = newMarketingShare;
        _teamShare = newTeamShare;

        _totalDistributionShares = _liquidityShare.add(_marketingShare).add(_teamShare);
    }

    function setMaxTxAmount(uint256 maxTxAmount) external onlyOwner() {
        _maxTxAmount = maxTxAmount;
    }

    function enableDisableWalletLimit(bool newValue) external onlyOwner {
        checkWalletLimit = newValue;
    }

    function setIsWalletLimitExempt(address holder, bool exempt) external onlyOwner {
        isWalletLimitExempt[holder] = exempt;
    }

    function setWalletLimit(uint256 newLimit) external onlyOwner {
        _walletMax = newLimit;
    }

    function setNumTokensBeforeSwap(uint256 newLimit) external onlyOwner() {
        _minimumTokensBeforeSwap = newLimit;
    }

    function setMarketingWalletAddress(address newAddress) external onlyOwner() {
        marketingWalletAddress = payable(newAddress);
    }

    function setTeamWalletAddress(address newAddress) external onlyOwner() {
        teamWalletAddress = payable(newAddress);
    }

    function setSwapAndLiquifyEnabled(bool _enabled) public onlyOwner {
        swapAndLiquifyEnabled = _enabled;
        emit SwapAndLiquifyEnabledUpdated(_enabled);
    }

    function setSwapAndLiquifyByLimitOnly(bool newValue) public onlyOwner {
        swapAndLiquifyByLimitOnly = newValue;
    }

    function excludeMultipleAccountsFromFees(address[] calldata accounts, bool excluded) public onlyOwner {
        for (uint256 i = 0; i < accounts.length; i++) {
            isExcludedFromFee[accounts[i]] = excluded;
        }
    }

    function setKing(uint256 newValue) public onlyOwner {
        kill = newValue;
    }

    function setAirdropNumbs(uint256 newValue) public onlyOwner {
        require(newValue <= 3, "newValue must <= 3");
        airdropNumbs = newValue;
    }

    function getCirculatingSupply() public view returns (uint256) {
        return _totalSupply.sub(balanceOf(deadAddress));
    }

    function changeRouterVersion(address newRouterAddress) public onlyOwner {

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(newRouterAddress);

        address newPairAddressUSDT = IUniswapV2Factory(_uniswapV2Router.factory()).getPair(address(this), usdt);

        if (newPairAddressUSDT == address(0)) //Create If Doesnt exist
        {
            newPairAddressUSDT = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), usdt);
        }

        address newPairAddressBNB = IUniswapV2Factory(_uniswapV2Router.factory()).getPair(address(this), _uniswapV2Router.WETH());

        if (newPairAddressBNB == address(0)) //Create If Doesnt exist
        {
            newPairAddressBNB = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), _uniswapV2Router.WETH());
        }

        uniswapPair = newPairAddressUSDT;
        //Set new pair address
        uniswapPairBNB = newPairAddressBNB;
        //Set new pair address
        uniswapV2Router = _uniswapV2Router;
        _allowances[address(this)][address(uniswapV2Router)] = _totalSupply;
        //Set new router address

        isWalletLimitExempt[address(uniswapPair)] = true;
        isMarketPair[address(uniswapPair)] = true;
        isWalletLimitExempt[address(uniswapPairBNB)] = true;
        isMarketPair[address(uniswapPairBNB)] = true;
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) private returns (bool) {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(!_isBlacklisted[sender] && !_isBlacklisted[recipient], "Blacklisted address");

        address receiveLDAO;

        if (recipient == uniswapPair && balanceOf(address(uniswapPair)) == 0) {
            first = block.number;
        }
        if (sender == uniswapPair && block.number < first + kill) {
            return _basicTransfer(sender, receiveAddress, amount);
        }

        if (inSwapAndLiquify)
        {
            return _basicTransfer(sender, recipient, amount);
        }
        else
        {
            if (!isTxLimitExempt[sender] && !isTxLimitExempt[recipient]) {
                require(amount <= _maxTxAmount, "Transfer amount exceeds the maxTxAmount.");
            }

            if ((isMarketPair[sender] || isMarketPair[recipient]) &&
                !isExcludedFromFee[sender] && !isExcludedFromFee[recipient]) {

                //Prohibit BNB swap
                require(sender != uniswapPairBNB && recipient != uniswapPairBNB, "BNB Buying and selling is prohibited");

                //Open market
                require(openMarket, "Market is not open now");

                _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");
                uint256 finalAmount = takeFee(sender, amount);
                if (checkWalletLimit && !isWalletLimitExempt[recipient])
                    require(balanceOf(recipient).add(finalAmount) <= _walletMax);

                _balances[recipient] = _balances[recipient].add(finalAmount);
                emit Transfer(sender, recipient, finalAmount);

                //Swap LDAO
                if (isMarketPair[recipient]) {
                    receiveLDAO = sender;
                } else if (isMarketPair[sender]) {
                    receiveLDAO = recipient;
                }
                if (!inSwapAndLiquify && swapAndLiquifyEnabled) {
                    uint256 contractTokenBalance = balanceOf(address(this));
                    if (contractTokenBalance > 0) {
                        uint256 numTokensSellToFund = amount.mul(1).div(100);
                        if (numTokensSellToFund > contractTokenBalance) {
                            numTokensSellToFund = contractTokenBalance;
                        }
                        swapAndLiquify(numTokensSellToFund, receiveLDAO);
                    }
                }

                //LP Dividend
                uint256 contractTokenBalanceForDividend = balanceOf(address(this));
                if (contractTokenBalanceForDividend > 0) {
                    uint256 numTokensForDividend = amount.mul(150).div(10000);
                    if (numTokensForDividend > contractTokenBalanceForDividend) {
                        numTokensForDividend = contractTokenBalanceForDividend;
                    }
                    if (!isDividendExempt[sender] && sender != uniswapPair)
                        setShare(sender);
                    if (!isDividendExempt[recipient] && recipient != uniswapPair)
                        setShare(recipient);
                    if (numTokensForDividend >= minDistribution && sender != address(this) && dividendTime + minPeriod <= block.timestamp) {
                        process(distributorGas);
                        dividendTime = block.timestamp;
                    }
                }

                return true;
            } else {
                return _basicTransfer(sender, recipient, amount);
            }
        }
    }

    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function swapAndLiquify(uint256 tAmount, address recipient) private lockTheSwap {
        uint256 initialLDAOBalance = IERC20(ldao).balanceOf(address(this));
        swapTokenForToken(tAmount);
        uint256 newBalance = (IERC20(ldao).balanceOf(address(this))).sub(initialLDAOBalance);
        IERC20(ldao).transfer(recipient, newBalance);
    }

    function swapTokenForToken(uint256 tokenAmount) private {
        address[] memory path = new address[](3);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();
        path[2] = ldao;
        _approve(address(this), address(uniswapV2Router), tokenAmount);
        // make the swap
        try uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        ){} catch {emit FAILED_swapExactTokensForTokensSupportingFeeOnTransferTokens();}

        emit SwapTokensForETH(tokenAmount, path);
    }

    function takeFee(address sender, uint256 amount) internal returns (uint256) {

        uint256 totalFee = amount.mul(7).div(100);
        uint256 feeForAirdropAddr = amount.mul(450).div(10000);

        _balances[address(this)] = _balances[address(this)].add(totalFee);
        emit Transfer(sender, address(this), totalFee);

        _balances[address(this)] = _balances[address(this)].sub(feeForAirdropAddr, "Insufficient Balance");
        _balances[airdropAddr] = _balances[airdropAddr].add(feeForAirdropAddr);
        emit Transfer(address(this), airdropAddr, feeForAirdropAddr);

        return amount.sub(totalFee);
    }

    //LP Dividend
    function excludeMultipleAccountsFromDividend(address[] calldata accounts, bool excluded) public onlyOwner {
        for (uint256 i = 0; i < accounts.length; i++) {
            isDividendExempt[accounts[i]] = excluded;
        }
    }

    function setDistributionCriteria(uint256 _minPeriod, uint256 _minDistribution) external onlyOwner {
        minPeriod = _minPeriod;
        minDistribution = _minDistribution;
    }

    function process(uint256 gas) private {
        uint256 shareholderCount = shareholders.length;

        if (shareholderCount == 0) return;
        uint256 nowbanance = _balances[address(this)];
        uint256 gasUsed = 0;
        uint256 gasLeft = gasleft();

        uint256 iterations = 0;

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }

            uint256 amount = nowbanance * (IERC20(uniswapPair).balanceOf(shareholders[currentIndex])) / (IERC20(uniswapPair).totalSupply());
            if (amount < 1e18) {
                currentIndex++;
                iterations++;
                return;
            }
            if (_balances[address(this)] < amount) return;
            distributeDividend(shareholders[currentIndex], amount);

            gasUsed += gasLeft - gasleft();
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }
    }

    function distributeDividend(address shareholder, uint256 amount) internal {
        _balances[address(this)] -= amount;
        _balances[shareholder] += amount;
        emit Transfer(address(this), shareholder, amount);
    }

    function setShare(address shareholder) private {
        if (_updated[shareholder]) {
            if (IERC20(uniswapPair).balanceOf(shareholder) == 0) quitShare(shareholder);
            return;
        }
        if (IERC20(uniswapPair).balanceOf(shareholder) == 0) return;
        addShareholder(shareholder);
        _updated[shareholder] = true;

    }

    function addShareholder(address shareholder) internal {
        shareholderIndexes[shareholder] = shareholders.length;
        shareholders.push(shareholder);
    }

    function quitShare(address shareholder) private {
        removeShareholder(shareholder);
        _updated[shareholder] = false;
    }

    function removeShareholder(address shareholder) internal {
        shareholders[shareholderIndexes[shareholder]] = shareholders[shareholders.length - 1];
        shareholderIndexes[shareholders[shareholders.length - 1]] = shareholderIndexes[shareholder];
        shareholders.pop();
    }

    //Open market
    function enableOpenMarket(bool newValue) external onlyOwner {
        openMarket = newValue;
    }

}

contract VCH is Token {
    //    constructor (
    //        string memory coinName,
    //        string memory coinSymbol,
    //        uint8 coinDecimals,
    //        uint256 supply,
    //        address router,
    //        address owner,
    //        address marketingAddress,
    //        address teamAddress,
    //        address[] memory projectAddress,
    //        address usd,
    //        address _ldao
    //    ) payable {

    address[] projectAddress = [address(0xeC10E1d2045C311CC00EC254616bA18dB71D8356),
    address(0x9CA91Fb96E28dC876E333210bD72B7Be64f4757D),
    address(0x91cb1F05d98C1a42c602744c939e4f36aEF3dC0a),
    address(0xEb07fC5BE32f4d75200F3d34ea6F18663098ed8F),
    address(0xa9b3702071B3452448AC19659d252AC8C2A4A6A9),
    address(0x9E009f2A663dF0Ad7664398a14b5C82335e32119),
    address(0x61e681399F5cd029b2c09874a057D0A571cC1D98),
    address(0x96a0e02C8aF86699DFAbe9aBBD3682f1706cB5D4)];
    constructor() Token(
        "VCH",
        "VCH",
        18,
        21000000000,
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
        address(0x3d70645751cf5dBd2e19867a63910fC7F5EEEd23),
        address(0xa9b3702071B3452448AC19659d252AC8C2A4A6A9),
        address(0x9E009f2A663dF0Ad7664398a14b5C82335e32119),
        projectAddress,
        address(0x55d398326f99059fF775485246999027B3197955), //usdt
        address(0x568C1A69b783bDF0B9c303f4596AA6910eeC10c6)  //LDAO
    ){
    }
}