/**
 Who controls the memes, Controls the universe.

 Twitter: @MemeRusher
 Telegram: t.me/MemeRusher

 Website: https://memerusher.com/

*/

// SPDX-License-Identifier: Unlicensed
pragma solidity 0.8.18;

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

library SafeMath {
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


}

abstract contract Context 
{
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}



abstract contract Ownable is Context 
{
    address internal _owner;
    address private _previousOwner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _owner = _msgSender();
        emit OwnershipTransferred(address(0), _owner);
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
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

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

interface IUniswapV2Pair {
    function factory() external view returns (address);
}

interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
}

interface IUniswapV2Router02 is IUniswapV2Router01 
{
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline) external payable returns (uint amountToken, uint amountETH, uint liquidity);
}

contract Memerusher is Context, IBEP20, Ownable {

    using SafeMath for uint256;
    mapping(address => uint256) private _rOwned;
    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;
    mapping(address => bool) private _isExcluded;
    mapping(address => bool) private _isExcludedFromMaxWallet;
    mapping(address => bool) private _isExcludedFromMaxTnxLimit;
    address[] private _excluded;

    
    address payable public marketingWallet;
    address public stakingWallet;
    address public _burnAddress = 0x000000000000000000000000000000000000dEaD;

    uint256 private constant MAX = ~uint256(0);
    uint256 private _tTotal;
    uint256 private _rTotal;
    uint256 private _tFeeTotal;
    string private _name;
    string private _symbol;
    uint256 private _decimals;
    uint256 private _totalFees;

    uint256 public _maxTxAmount;
    uint256 public _maxWalletBalance;

    uint256 private _buyRefFee = 1;   
    uint256 private _buyLiquidityAndMarketingFee = 4; // Liquidity + Marketing Fee  
    uint256 private _buyStakingFee = 0;

    uint256 private _sellRefFee = 1;
    uint256 private _sellLiquidityAndMarketingFee = 4;  // Liquidity + Marketing Fee 
    uint256 private _sellStakingFee = 0;

    uint256 public marketingDivisor = 2;   // marketing part out of liquidityAndMarketingFee

    uint256 public _refFee = _buyRefFee;
    uint256 public _liquidityAndMarketingFee = _buyLiquidityAndMarketingFee; //liquidity and marketing fee 
    uint256 public _stakingFee = _buyStakingFee;

    uint256 private _previousRefFee = _refFee;
    uint256 private _previousLiquidityAndMarketing = _liquidityAndMarketingFee;
    uint256 private _previousStakingFee = _stakingFee;

    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapV2Pair;
    bool inSwapAndSendFee;
    bool public swapAndSendFeeEnabled = true;
    uint256 public numTokensSellToSendFee;
    event SwapAndSendFeeEnabledUpdated(bool enabled);
   
    modifier lockTheSwap() {
        inSwapAndSendFee = true;
        _;
        inSwapAndSendFee = false;
    }

    bool inSwapAndLiquify;
    bool public swapAndLiquifyEnabled = false;
	event SwapAndLiquifyEnabledUpdated(bool enabled);
    event SwapAndLiquify(uint256 tokensSwapped, uint256 ethReceived, uint256 tokensIntoLiqudity);


    constructor() 
    {
        _name = "MEMERUSHER";
        _symbol = "RUSHER";
        _decimals = 9;
        _tTotal = 100000000 * 10**_decimals; 
        _rTotal = (MAX - (MAX % _tTotal));
        numTokensSellToSendFee = 1000 * 10**_decimals; 
        _maxTxAmount = 500000 * 10**_decimals; //0.5%  
        _maxWalletBalance = 500000 * 10**_decimals; //0.5%  
        marketingWallet = payable(0xc8e45CaD51450A327122392bf2cDb02074F6ef5B);
        stakingWallet = 0x60383149A0c658d1e8Af3D8a7a12794aD86e46b5;

        _rOwned[_msgSender()] = _rTotal;

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), _uniswapV2Router.WETH());

        uniswapV2Router = _uniswapV2Router;

        _isExcludedFromFee[_msgSender()] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[stakingWallet] = true;
        _isExcludedFromFee[marketingWallet] = true;

        _isExcluded[_burnAddress] = true;
        _isExcluded[uniswapV2Pair] = true;

        _isExcludedFromMaxTnxLimit[owner()] = true;
        _isExcludedFromMaxTnxLimit[address(this)] = true;
        _isExcludedFromMaxTnxLimit[marketingWallet] = true;
        _isExcludedFromMaxTnxLimit[stakingWallet] = true;

        _isExcludedFromMaxWallet[owner()] = true;
        _isExcludedFromMaxWallet[address(this)] = true;
        _isExcludedFromMaxWallet[marketingWallet] = true;
        _isExcludedFromMaxWallet[stakingWallet] = true;

        _owner = _msgSender();
        emit Transfer(address(0), _msgSender(), _tTotal);
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint256) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        if (_isExcluded[account]) return _tOwned[account];
        return tokenFromReflection(_rOwned[account]);
    }

    function transfer(address recipient, uint256 amount)
        public
        override
        returns (bool)
    {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender)
        public
        view
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount)
        public
        override
        returns (bool)
    {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()].sub(
                amount,
                "BEP20: transfer amount exceeds allowance"
            )
        );
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].add(addedValue)
        );
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].sub(
                subtractedValue,
                "BEP20: decreased allowance below zero"
            )
        );
        return true;
    }

    function isExcludedFromReward(address account) public view returns (bool) {
        return _isExcluded[account];
    }

    function totalFees() public view returns (uint256) {
        return _tFeeTotal;
    }

    function deliver(uint256 tAmount) public {
        address sender = _msgSender();
        require(
            !_isExcluded[sender],
            "Excluded addresses cannot call this function"
        );
        (uint256 rAmount, , , , , , ) = _getValues(tAmount);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        _rTotal = _rTotal.sub(rAmount);
        _tFeeTotal = _tFeeTotal.add(tAmount);
    }

    function reflectionFromToken(uint256 tAmount, bool deductTransferFee)
        public
        view
        returns (uint256)
    {
        require(tAmount <= _tTotal, "Amount must be less than supply");
        if (!deductTransferFee) {
            (uint256 rAmount, , , , , , ) = _getValues(tAmount);
            return rAmount;
        } else {
            (, uint256 rTransferAmount, , , , , ) = _getValues(tAmount);
            return rTransferAmount;
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
        return rAmount.div(currentRate);
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
        require(_isExcluded[account], "Account is already included");
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

    function _transferBothExcluded(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        (
            uint256 rAmount,
            uint256 rTransferAmount,
            uint256 rFee,
            uint256 tTransferAmount,
            uint256 tFee,
            uint256 tMarketing,
            uint256 tStaking
        ) = _getValues(tAmount);
        _tOwned[sender] = _tOwned[sender].sub(tAmount);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        _tOwned[recipient] = _tOwned[recipient].add(tTransferAmount);
        _rOwned[recipient] = _rOwned[recipient].add(rTransferAmount);
        _takeMarketing(tMarketing);
        _takeStaking(tStaking);
        _reflectFee(rFee, tFee);
        emit Transfer(sender, recipient, tTransferAmount);
    }

    function excludeFromFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = true;
    }

    function includeInFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = false;
    }

     function setMaxTxAmount(uint256 maxTxAmount) external onlyOwner {
        _maxTxAmount = maxTxAmount * 10**_decimals;
    }

    function includeAndExcludedFromMaxWallet(address account, bool value) public onlyOwner {
            _isExcludedFromMaxWallet[account] = value;
    }

    function includeAndExcludedFromMaxTnxLimit(address account, bool value) public onlyOwner {
        _isExcludedFromMaxTnxLimit[account] = value;
    }

    function isExcludedFromMaxWallet(address account) public view returns(bool){
            return _isExcludedFromMaxWallet[account];
    }

    function isExcludedFromMaxTnxLimit(address account) public view returns(bool) {
        return _isExcludedFromMaxTnxLimit[account];
    }

    function setMarketingWalletAddress(address _addr) external onlyOwner {
        marketingWallet = payable(_addr);
    }

    function setStakingWalletAddress(address _addr) external onlyOwner {
        stakingWallet = _addr;
    }

     function setSellFeePercent(
        uint256 rFee,
        uint256 lmFee,
        uint256 sFee
    ) external onlyOwner {
        uint256 fees = rFee.add(lmFee.add(sFee));
        require(fees <= 30, "BEP20: Maximum Sell fees allowed is 30%");
        _sellRefFee = rFee;
        _refFee = _sellRefFee;
        _sellLiquidityAndMarketingFee = lmFee;
        _liquidityAndMarketingFee = _sellLiquidityAndMarketingFee;
        _sellStakingFee = sFee;
        _stakingFee = _sellStakingFee;

    }

    function setBuyFeePercent(
        uint256 rFee,
        uint256 lmFee,
        uint256 sFee
    ) external onlyOwner {
        uint256 fees = rFee.add(lmFee.add(sFee));
        require(fees <= 30, "BEP20: Maximum buy fees allowed is 30%");
        _buyRefFee = rFee;
        _refFee = _buyRefFee;
        _buyLiquidityAndMarketingFee = lmFee;
        _liquidityAndMarketingFee = _buyLiquidityAndMarketingFee;
        _buyStakingFee = sFee;
        _stakingFee = _buyStakingFee;
    }


    function setNumTokensSellToSendFee(uint256 amount)  external onlyOwner
    {
        numTokensSellToSendFee = amount * 10**_decimals;
    }

    function setRouterAddress(address newRouter) external onlyOwner {
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(newRouter);
        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), _uniswapV2Router.WETH());
        uniswapV2Router = _uniswapV2Router;
    }

    function setSwapAndSendFeeEnabled(bool _enabled) external onlyOwner {
        swapAndSendFeeEnabled = _enabled;
        emit SwapAndSendFeeEnabledUpdated(_enabled);
    }

    receive() external payable {}

    function _reflectFee(uint256 rFee, uint256 tFee) private {
        _rTotal = _rTotal.sub(rFee);
        _tFeeTotal = _tFeeTotal.add(tFee);
    }

    function _getValues(uint256 tAmount)
        private
        view
        returns (
            uint256,
            uint256,
            uint256,
            uint256,
            uint256,
            uint256,
            uint256
        )
    {
        (
            uint256 tTransferAmount,
            uint256 tFee,
            uint256 tMarketing,
            uint256 tStaking
        ) = _getTValues(tAmount);
        (uint256 rAmount, uint256 rTransferAmount, uint256 rFee) = _getRValues(
            tAmount,
            tFee,
            tMarketing,
            tStaking,
            _getRate()
        );
        return (
            rAmount,
            rTransferAmount,
            rFee,
            tTransferAmount,
            tFee,
            tMarketing,
            tStaking
        );
    }

    function _getTValues(uint256 tAmount)
        private
        view
        returns (
            uint256,
            uint256,
            uint256,
            uint256
        )
    {
        uint256 tFee = calculateTaxFee(tAmount);
        uint256 tMarketing = calculatemarketingAndLiquidity(tAmount);
        uint256 tStaking = calculateStakingFee(tAmount);
        uint256 tTransferAmount = tAmount.sub(tFee).sub(tMarketing).sub(
            tStaking
        );
        return (tTransferAmount, tFee, tMarketing, tStaking);
    }

    function _getRValues(
        uint256 tAmount,
        uint256 tFee,
        uint256 tMarketing,
        uint256 tStaking,
        uint256 currentRate
    )
        private
        pure
        returns (
            uint256,
            uint256,
            uint256
        )
    {
        uint256 rAmount = tAmount.mul(currentRate);
        uint256 rFee = tFee.mul(currentRate);
        uint256 rMarketing = tMarketing.mul(currentRate);
        uint256 rStaking = tStaking.mul(currentRate);
        uint256 rTransferAmount = rAmount.sub(rFee).sub(rMarketing).sub(
            rStaking
        );
        return (rAmount, rTransferAmount, rFee);
    }

    function _getRate() private view returns (uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply.div(tSupply);
    }

    function _getCurrentSupply() private view returns (uint256, uint256) {
        uint256 rSupply = _rTotal;
        uint256 tSupply = _tTotal;
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (
                _rOwned[_excluded[i]] > rSupply ||
                _tOwned[_excluded[i]] > tSupply
            ) return (_rTotal, _tTotal);
            rSupply = rSupply.sub(_rOwned[_excluded[i]]);
            tSupply = tSupply.sub(_tOwned[_excluded[i]]);
        }
        if (rSupply < _rTotal.div(_tTotal)) return (_rTotal, _tTotal);
        return (rSupply, tSupply);
    }

    function _takeMarketing(uint256 tMarketing)
        private
    {
        uint256 currentRate = _getRate();
        uint256 rMarketingAndMarketing = tMarketing.mul(currentRate
        );
        _rOwned[address(this)] = _rOwned[address(this)].add(
            rMarketingAndMarketing
        );
        if (_isExcluded[address(this)])
            _tOwned[address(this)] = _tOwned[address(this)].add(
                tMarketing
            );
    }



    function _takeStaking(uint256 tStaking) private {
        uint256 currentRate = _getRate();
        uint256 rStaking = tStaking.mul(currentRate);
        _rOwned[stakingWallet] = _rOwned[stakingWallet].add(
            rStaking
        );
        if (_isExcluded[stakingWallet])
            _tOwned[stakingWallet] = _tOwned[stakingWallet].add(
                tStaking
            );
    }

    function calculateTaxFee(uint256 _amount) private view returns (uint256) {
        return _amount.mul(_refFee).div(10**2);
    }

    function calculateStakingFee(uint256 _amount)
        private
        view
        returns (uint256)
    {
        return _amount.mul(_stakingFee).div(10**2);
    }

    function calculatemarketingAndLiquidity(uint256 _amount)
        private
        view
        returns (uint256)
    {
        return _amount.mul((_liquidityAndMarketingFee)).div(10**2);
    }

    function removeAllFee() private {
        _previousRefFee = _refFee;
        _previousLiquidityAndMarketing = _liquidityAndMarketingFee;
        _previousStakingFee = _stakingFee;

        _refFee = 0;
        _liquidityAndMarketingFee = 0;
        _stakingFee = 0;
    }

    function restoreAllFee() private {
        _refFee = _previousRefFee;
        _liquidityAndMarketingFee = _previousLiquidityAndMarketing;
        _stakingFee = _previousStakingFee;
    }

    function isExcludedFromFee(address account) public view returns (bool) {
        return _isExcludedFromFee[account];
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
        require(amount > 0, "Transfer amount must be greater than zero");
        
        if (from != owner() && to != owner())
            require( _isExcludedFromMaxTnxLimit[from] || _isExcludedFromMaxTnxLimit[to] || 
                amount <= _maxTxAmount,
                "BEP20: Transfer amount exceeds the maxTxAmount."
            );
    
        if (
            from != owner() &&
            to != address(this) &&
            to != _burnAddress &&
            to != uniswapV2Pair ) 
        {
            uint256 currentBalance = balanceOf(to);
            require(_isExcludedFromMaxWallet[to] || (currentBalance + amount <= _maxWalletBalance),
                    "BEP20: Reached max wallet holding");
        }
    
        uint256 contractTokenBalance = balanceOf(address(this));

        bool overMinTokenBalance = contractTokenBalance >=
            numTokensSellToSendFee;
        if (overMinTokenBalance &&  !inSwapAndSendFee &&  from != uniswapV2Pair &&  swapAndSendFeeEnabled) 
        {
            contractTokenBalance = numTokensSellToSendFee;
            swapAndLiquify(contractTokenBalance);
        }

        bool takeFee = true;
        if (_isExcludedFromFee[from] || _isExcludedFromFee[to]) {
            takeFee = false;
        } else {

            if (from == uniswapV2Pair) {

                _refFee = _buyRefFee;
                _liquidityAndMarketingFee = _buyLiquidityAndMarketingFee;
                _stakingFee = _buyStakingFee;
            } else if (to == uniswapV2Pair) {
                _refFee = _sellRefFee;
                _liquidityAndMarketingFee = _sellLiquidityAndMarketingFee;
                _stakingFee = _sellStakingFee;
            } else {
                _refFee = 0;
                _liquidityAndMarketingFee = 0;
                _stakingFee = 0;
            }
        }
        _tokenTransfer(from, to, amount, takeFee);
    }

    function swapAndLiquify(uint256 contractTokenBalance) private lockTheSwap 
    {
        uint256 halfLiquidityTokens = contractTokenBalance.mul(_liquidityAndMarketingFee-marketingDivisor).div(_liquidityAndMarketingFee).div(2);
        uint256 swapableTokens = contractTokenBalance.sub(halfLiquidityTokens);
        swapTokensForEth(swapableTokens); 
        uint256 ethBalance = address(this).balance;
        uint256 ethForLiquidity =  ethBalance.mul(_liquidityAndMarketingFee-marketingDivisor).div(_liquidityAndMarketingFee).div(2);
         addLiquidity(halfLiquidityTokens, ethForLiquidity);
         emit SwapAndLiquify(halfLiquidityTokens, ethForLiquidity, swapableTokens);
         if((ethBalance-ethForLiquidity)>0)
         {
             marketingWallet.transfer(ethBalance-ethForLiquidity);
         }        
    }


    function addLiquidity(uint256 tokenAmount, uint256 ethAmount) private {
        // approve token transfer to cover all possible scenarios
        _approve(address(this), address(uniswapV2Router), tokenAmount);

        // add the liquidity
        uniswapV2Router.addLiquidityETH{value: ethAmount}(
            address(this),
            tokenAmount,
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            owner(),
            block.timestamp
        );
    }


    function swapTokensForEth(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();
        _approve(address(this), address(uniswapV2Router), tokenAmount);
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of ETH
            path,
            address(this),
            block.timestamp
        );
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 amount,
        bool takeFee
    ) private {
        if (!takeFee) removeAllFee();

        if (_isExcluded[sender] && !_isExcluded[recipient]) {
            _transferFromExcluded(sender, recipient, amount);
        } else if (!_isExcluded[sender] && _isExcluded[recipient]) {
            _transferToExcluded(sender, recipient, amount);
        } else if (!_isExcluded[sender] && !_isExcluded[recipient]) {
            _transferStandard(sender, recipient, amount);
        } else if (_isExcluded[sender] && _isExcluded[recipient]) {
            _transferBothExcluded(sender, recipient, amount);
        } else {
            _transferStandard(sender, recipient, amount);
        }

        if (!takeFee) restoreAllFee();
    }

    function _transferStandard(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        (
            uint256 rAmount,
            uint256 rTransferAmount,
            uint256 rFee,
            uint256 tTransferAmount,
            uint256 tFee,
            uint256 tMarketing,
            uint256 tStaking
        ) = _getValues(tAmount);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        _rOwned[recipient] = _rOwned[recipient].add(rTransferAmount);
        _takeMarketing(tMarketing);
        _takeStaking(tStaking);
        _reflectFee(rFee, tFee);
        emit Transfer(sender, recipient, tTransferAmount);
    }

    function _transferToExcluded(address sender, address recipient, uint256 tAmount) private {
        (
            uint256 rAmount,
            uint256 rTransferAmount,
            uint256 rFee,
            uint256 tTransferAmount,
            uint256 tFee,
            uint256 tMarketing,
            uint256 tStaking
) = _getValues(tAmount);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        _tOwned[recipient] = _tOwned[recipient].add(tTransferAmount);
        _rOwned[recipient] = _rOwned[recipient].add(rTransferAmount);
        _takeMarketing(tMarketing);
        _takeStaking(tStaking);
        _reflectFee(rFee, tFee);
        emit Transfer(sender, recipient, tTransferAmount);
    }

    function _transferFromExcluded(address sender, address recipient, uint256 tAmount) private 
    {
        (uint256 rAmount, uint256 rTransferAmount, uint256 rFee, uint256 tTransferAmount, uint256 tFee, uint256 tMarketing, uint256 tStaking) = _getValues(tAmount);
        _tOwned[sender] = _tOwned[sender].sub(tAmount);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        _rOwned[recipient] = _rOwned[recipient].add(rTransferAmount);
        _takeMarketing(tMarketing);
        _takeStaking(tStaking);
        _reflectFee(rFee, tFee);
        emit Transfer(sender, recipient, tTransferAmount);
    }


    function setMarketingDivisor(uint256 _marketingDivisor) external onlyOwner
    {
        require(marketingDivisor<_liquidityAndMarketingFee && marketingDivisor>=0, "Divisor must be greater than zero and less than _liquidityAndMarketingFee");
        marketingDivisor = _marketingDivisor;
    }
    
}