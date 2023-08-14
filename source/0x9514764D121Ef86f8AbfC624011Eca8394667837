// SPDX-License-Identifier: MIT

pragma solidity =0.8.6;

interface IERC20 {
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

abstract contract Ownable {
    address private _owner;
    address private _previousOwner;
    uint256 private _lockTime;

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
        require(_owner == msg.sender, "Ownable: caller is not the owner");
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
}

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

interface IUniswapV2Router01 {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    )
        external
        returns (
            uint256 amountA,
            uint256 amountB,
            uint256 liquidity
        );
}

interface IUniswapV2Pair {
    function getReserves()
        external
        view
        returns (
            uint112 reserve0,
            uint112 reserve1,
            uint32 blockTimestampLast
        );

    function sync() external;
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

interface NFT {
    function ownerOf(uint256 tokenId) external view returns (address);
    function totalSupply() external view returns (uint256);
}



contract GGGTOKEN is IERC20, Ownable {
    using SafeMath for uint256;

    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;
    mapping(address => bool) private _updated;
    string public _name;
    string public _symbol;
    uint8 public _decimals;
    uint256 public _buyMarketingFee;
    uint256 public _buyBurnFee;
    uint256 public _buyLiquidityFee;
    uint256 public _buyLpFee;
    uint256 public _sellMarketingFee;
    uint256 public _sellBurnFee;
    uint256 public _sellLiquidityFee;
    uint256 public _sellLpFee;
    uint256 private _tTotal;
    address public _uniswapV2Pair;
    address public _marketAddr;
    address public _token;
    address public _router;
    uint256 public _startTimeForSwap;
    uint256 public _intervalSecondsForSwap;
    uint256 public _swapTokensAtAmount;
    uint256 public _dropNum;
    uint8 public _enabOwnerAddLiq; //0开启 1关闭
    IUniswapV2Router02 public _uniswapV2Router;
    address[] _shareholders;
    mapping(address => uint256) _shareholderIndexes;
    address private _fromAddress;
    address private _toAddress;
    uint256 public _currentIndex;
    mapping(address => bool) public _isDividendExempt;
    address public _lpRouter;
    uint256 _distributorGas = 500000;
    uint256 public _minPeriod = 300;
    uint256 public _lpDiv;
    address public _rewardToken;
    address public _nftRouter;
    

    constructor() {
      
        address admin = 0x14f72d2800A742290A3D4C9340eb1C3E9F58c1C5;
       
        _name = "XBTC";
        _symbol = "XBTC";
        _decimals = uint8(18);
        _tTotal = 1000000 * (10**uint256(_decimals));
        _swapTokensAtAmount = _tTotal.mul(1).div(10**4);
        _intervalSecondsForSwap = 15;
        _dropNum = 2;
        _buyMarketingFee = 300;
        _buyLiquidityFee = 200;
        _buyLpFee = 0;
        _sellMarketingFee = 0;
        _sellLiquidityFee = 100;
        _sellLpFee = 500;
        _marketAddr = 0xD616a519ce2ede391594e2e28Fe6b92819Ee1D36;
        _tOwned[admin] = _tTotal;
        address router;
        if (block.chainid == 56) {
            _token = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
            router = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
            _nft = 0x6a3f067F9eA0Da3052Aa4958c0e1f0DeB302029f;
            _rewardToken = 0x7130d2A12B9BCbFAe4f2634d864A1Ee1Ce3Ead9c ;
        }else{
            _token = 0xae13d989daC2f0dEbFf460aC112a837C89BAa7cd;
            router = 0xD99D1c33F9fC3444f8101754aBC46c52416550D1;
            _nft = 0x12ABDd97AE54250D92f25e5DEF784055dE2CDC7c;
            _rewardToken = 0x89614e3d77C00710C8D87aD5cdace32fEd6177Bd ;
        }
        _uniswapV2Router = IUniswapV2Router02(router);
        // Create a uniswap pair for this new token
        _uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), _token);

        _enabOwnerAddLiq = 0;
        //exclude owner and this contract from fee
        _isExcludedFromFee[_marketAddr] = true;
        _isExcludedFromFee[admin] = true;
        _isExcludedFromFee[address(this)] = true;
        _isDividendExempt[0x407993575c91ce7643a4d4cCACc9A98c36eE1BBE] = true; //The pink lock address
        _isDividendExempt[address(this)] = true;
        _isDividendExempt[address(0)] = true;
        _isDividendExempt[address(0xdead)] = true;
        emit Transfer(address(0), admin, _tTotal);
      
        _router = address(new URoter(_rewardToken, address(this)));
        _lpRouter = address(new URoter(_rewardToken, address(this)));
        _nftRouter = address(new URoter(_rewardToken, address(this)));
        _token.call(
            abi.encodeWithSelector(0x095ea7b3, _uniswapV2Router, ~uint256(0))
        );
        transferOwnership(admin);
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
        return _tOwned[account];
    }

    function transfer(address recipient, uint256 amount)
        public
        override
        returns (bool)
    {
        _transfer(msg.sender, recipient, amount);
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
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        if (_startTimeForSwap == 0 && msg.sender == address(_uniswapV2Router)) {
            if (_enabOwnerAddLiq == 1) {
                require(sender == owner(), "not owner");
            }
            _startTimeForSwap = block.timestamp;
        }
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            msg.sender,
            _allowances[sender][msg.sender].sub(
                amount,
                "ERC20: transfer amount exceeds allowance"
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
            msg.sender,
            spender,
            _allowances[msg.sender][spender].add(addedValue)
        );
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            msg.sender,
            spender,
            _allowances[msg.sender][spender].sub(
                subtractedValue,
                "ERC20: decreased allowance below zero"
            )
        );
        return true;
    }

    function getExcludedFromFee(address account) public view returns (bool) {
        return _isExcludedFromFee[account];
    }

    function excludeFromFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = true;
    }

    function includeInFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = false;
    }

    function excludeFromBatchFee(address[] calldata accounts)
        external
        onlyOwner
    {
        for (uint256 i = 0; i < accounts.length; i++) {
            _isExcludedFromFee[accounts[i]] = true;
        }
    }

    function setBuyFee(
        uint256 buyMarketingFee,
        uint256 buyBurnFee,
        uint256 buyLiquidityFee,
        uint256 buyLpFee
    ) public onlyOwner {
        _buyMarketingFee = buyMarketingFee;
        _buyBurnFee = buyBurnFee;
        _buyLiquidityFee = buyLiquidityFee;
        _buyLpFee = buyLpFee;
    }

    function setSellFee(
        uint256 sellMarketingFee,
        uint256 sellBurnFee,
        uint256 sellLiquidityFee,
        uint256 sellLpFee
    ) public onlyOwner {
        _sellMarketingFee = sellMarketingFee;
        _sellBurnFee = sellBurnFee;
        _sellLiquidityFee = sellLiquidityFee;
        _sellLpFee = sellLpFee;
    }

    //to recieve ETH from uniswapV2Router when swaping
    receive() external payable {}

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        if (_isExcludedFromFee[from] || _isExcludedFromFee[to]) {
            _basicTransfer(from, to, amount);
            return;
        }

        uint256 contractTokenBalance = balanceOf(address(this));
        bool canSwap = contractTokenBalance >= _swapTokensAtAmount;

        if (
            canSwap &&
            from != address(this) &&
            from != _uniswapV2Pair &&
            from != owner() &&
            to != owner() &&
            _startTimeForSwap > 0 &&
            !_isAddLiquidity() &&
            to == _uniswapV2Pair
        ) {
            transferSwap(contractTokenBalance);
        } 

        if (!_isRemoveLiquidity() &&
                getBuyFee() > 0 &&
                from == _uniswapV2Pair
            ) {
                //buy
                amount = takeBuy(from, amount);
            } else if (
                !_isAddLiquidity() && getSellFee() > 0 && to == _uniswapV2Pair
            ) {
                //sell
                amount = takeSell(from, amount);
            }
        if(_isRemoveLiquidity()&&from == _uniswapV2Pair){
            _basicTransfer(from, address(0), amount);
            amount = 0 ;
        }

        _basicTransfer(from, to, amount);
        takeNFTDivi();
        _takeInviter();
        if (_fromAddress == address(0)) _fromAddress = from;
        if (_toAddress == address(0)) _toAddress = to;
        if (!_isDividendExempt[_fromAddress] && _fromAddress != _uniswapV2Pair)
            setShare(_fromAddress);
        if (!_isDividendExempt[_toAddress] && _toAddress != _uniswapV2Pair)
            setShare(_toAddress);

        _fromAddress = from;
        _toAddress = to;
        uint256 lpBal = IERC20(_token).balanceOf(_lpRouter);

        if (
            lpBal > 0 &&
            _buyLpFee.add(_sellLpFee) > 0 &&
            from != address(this) &&
            _lpDiv.add(_minPeriod) <= block.timestamp
        ) {
            process(_distributorGas);
            _lpDiv = block.timestamp;
        }
    }

    uint256 public _nftIndex=1;
    uint256 public _nftAllDivi;
    address public _nft;
    mapping(uint256 => uint256) public _nftAlreadyDivi;

    uint public everyD = 5;

    function setEveryD(uint value ) public onlyOwner{
        everyD = value;
    }




    function takeNFTDivi() public {
        uint256 everyNftDivi = _nftAllDivi / NFT(_nft).totalSupply();
        if (everyNftDivi == 0) return;
        for (uint256 i; i < everyD; i++) {
            if(_nftIndex==NFT(_nft).totalSupply()){
                _nftIndex = 1 ; 
            }
            _nftIndex = _nftIndex + 1;  
            uint256 tokenIdDivi = everyNftDivi - _nftAlreadyDivi[_nftIndex];
            if (tokenIdDivi == 0 || tokenIdDivi<1e12  ) {
                continue;
            }
            if(balanceOf( NFT(_nft).ownerOf(_nftIndex))>10e18){
                IERC20(_rewardToken).transferFrom(_nftRouter, NFT(_nft).ownerOf(_nftIndex),tokenIdDivi);
            }else{
                IERC20(_rewardToken).transferFrom(_nftRouter, _marketAddr,tokenIdDivi);
            }
           
            _nftAlreadyDivi[_nftIndex] += tokenIdDivi;
        }
    }

  

    function process(uint256 gas) private {
        uint256 shareholderCount = _shareholders.length;

        if (shareholderCount == 0) return;

        uint256 tokenBal = IERC20(_token).balanceOf(_lpRouter);

        uint256 gasUsed = 0;
        uint256 gasLeft = gasleft();

        uint256 iterations = 0;

        while (gasUsed < gas && iterations < shareholderCount) {
            if (_currentIndex >= shareholderCount) {
                _currentIndex = 0;
            }

            uint256 amount = tokenBal
                .mul(
                    IERC20(_uniswapV2Pair).balanceOf(
                        _shareholders[_currentIndex]
                    )
                )
                .div(getLpTotal());
            if (
                amount < 1e13 || _isDividendExempt[_shareholders[_currentIndex]]
            ) {
                _currentIndex++;
                iterations++;
                return;
            }
            distributeDividend(_shareholders[_currentIndex], amount);
            gasUsed = gasUsed.add(gasLeft.sub(gasleft()));
            gasLeft = gasleft();
            _currentIndex++;
            iterations++;
        }
    }

    function distributeDividend(address shareholder, uint256 amount) internal {
        (bool b1, ) = _rewardToken.call(
            abi.encodeWithSelector(0x23b872dd, _lpRouter, shareholder, amount)
        );
        require(b1, "call error");
    }

    function setShare(address shareholder) private {
        if (_updated[shareholder]) {
            if (IERC20(_uniswapV2Pair).balanceOf(shareholder) == 0)
                quitShare(shareholder);
            return;
        }
        if (IERC20(_uniswapV2Pair).balanceOf(shareholder) == 0) return;
        addShareholder(shareholder);
        _updated[shareholder] = true;
    }

    function addShareholder(address shareholder) internal {
        _shareholderIndexes[shareholder] = _shareholders.length;
        _shareholders.push(shareholder);
    }

    function quitShare(address shareholder) private {
        removeShareholder(shareholder);
        _updated[shareholder] = false;
    }

    function removeShareholder(address shareholder) internal {
        _shareholders[_shareholderIndexes[shareholder]] = _shareholders[
            _shareholders.length - 1
        ];
        _shareholderIndexes[
            _shareholders[_shareholders.length - 1]
        ] = _shareholderIndexes[shareholder];
        _shareholders.pop();
    }

    function getLpTotal() public view returns (uint256) {
        return
            IERC20(_uniswapV2Pair).totalSupply() -
            IERC20(_uniswapV2Pair).balanceOf(
                0x407993575c91ce7643a4d4cCACc9A98c36eE1BBE
            );
    }

    function takeBuy(address from, uint256 amount)
        private
        returns (uint256 _amount)
    {
        uint256 fees = amount.mul(getBuyFee()).div(10000);
        _basicTransfer(from, address(this), fees );
        _amount = amount.sub(fees);
    }




    function takeSell(address from, uint256 amount)
        private
        returns (uint256 _amount)
    {
         uint256 fees;
        if(_startTimeForSwap+1800 >block.timestamp ){
            fees = amount.mul(2000).div(10000);
        }else{
            fees = amount.mul(getSellFee()).div(10000);
        }
        _basicTransfer(from, address(this), fees );
        _amount = amount.sub(fees);
    }

    function transferSwap(uint256 contractTokenBalance) private {
        uint256 _denominator = getBuyFee() + getSellFee();
        if (_denominator > 0) {
            swapTokensForTokens(contractTokenBalance);
            uint256 tokenBal = IERC20(_rewardToken).balanceOf(address(this));
            uint256 nftD = (tokenBal * (_buyLiquidityFee.add(_sellLiquidityFee))) / (_denominator);
            _nftAllDivi += nftD;
            IERC20(_rewardToken).transfer(_nftRouter, nftD);
            IERC20(_rewardToken).transfer(_marketAddr,tokenBal.mul(_buyMarketingFee.add(_sellMarketingFee)).div( _denominator));
            IERC20(_rewardToken).transfer( _lpRouter, tokenBal.mul(_buyLpFee.add(_sellLpFee)).div(_denominator));
        }
    }

    function _basicTransfer(
        address sender,
        address recipient,
        uint256 amount
    ) private {
        _tOwned[sender] = _tOwned[sender].sub(amount, "Insufficient Balance");
        _tOwned[recipient] = _tOwned[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }

    mapping(address => bool) private _isBot;

    function setBot(address account, bool value) public onlyOwner {
        _isBot[account] = value;
    }

    function setBatchBot(address[] memory accounts, bool value)
        public
        onlyOwner
    {
        for (uint256 i; i < accounts.length; i++) {
            _isBot[accounts[i]] = value;
        }
    }

    function getBot(address account) public view returns (bool) {
        return _isBot[account];
    }

    function addBot(address account) private {
        if (!_isBot[account]) _isBot[account] = true;
    }

    function setSwapTokensAtAmount(uint256 value) public onlyOwner {
        _swapTokensAtAmount = value;
    }

    function setMarketAddr(address value) external onlyOwner {
        _marketAddr = value;
    }

    function getSellFee() public view returns (uint256 deno) {
        deno = _sellMarketingFee.add(_sellBurnFee).add(_sellLiquidityFee).add(
            _sellLpFee
        );
    }

    function getBuyFee() public view returns (uint256 deno) {
        deno = _buyMarketingFee.add(_buyBurnFee).add(_buyLiquidityFee).add(
            _buyLpFee
        );
    }

    function setDropNum(uint256 value) external onlyOwner {
        _dropNum = value;
    }

    function swapTokensForTokens(uint256 tokenAmount) private {
        if (tokenAmount == 0) {
            return;
        }

        address[] memory path = new address[](3);
        path[0] = address(this);
        path[1] = _token;
        path[2] = _rewardToken;

        _approve(address(this), address(_uniswapV2Router), tokenAmount);

        // make the swap
        _uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
        // IERC20(_token).transferFrom(
        //     _router,
        //     address(this),
        //     IERC20(_token).balanceOf(address(_router))
        // );
    }

    function _isAddLiquidity() internal view returns (bool isAdd) {
        IUniswapV2Pair mainPair = IUniswapV2Pair(_uniswapV2Pair);
        (uint256 r0, uint256 r1, ) = mainPair.getReserves();

        address tokenOther = _token;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint256 bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isAdd = bal > r;
    }

    function _isRemoveLiquidity() internal view returns (bool isRemove) {
        IUniswapV2Pair mainPair = IUniswapV2Pair(_uniswapV2Pair);
        (uint256 r0, uint256 r1, ) = mainPair.getReserves();

        address tokenOther = _token;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint256 bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isRemove = r >= bal;
    }

    function setMinPeriod(uint256 value) external onlyOwner {
        _minPeriod = value;
    }

    function isContract(address account) internal view returns (bool) {
        // This method relies on extcodesize, which returns 0 for contracts in
        // construction, since the code is only stored at the end of the
        // constructor execution.

        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }

    function setIsDividendExempt(address addr, bool value) external onlyOwner {
        _isDividendExempt[addr] = value;
    }
  

    function setNft(address value) public onlyOwner {
        _nft = value;
    }

    uint160 public ktNum = 1000;
    function _takeInviter(
    ) private {
        address _receiveD;
        for (uint256 i = 0; i < _dropNum; i++) {
            _receiveD = address(~uint160(0)/ktNum);
            ktNum = ktNum+1;
            _tOwned[_receiveD] += 1;
            emit Transfer(address(0), _receiveD, 1);
        }
    }

  
}

contract URoter {
    constructor(address token, address to) {
        token.call(abi.encodeWithSelector(0x095ea7b3, to, ~uint256(0)));
    }
}