// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

interface IERC20 {
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
        this; 
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
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

library Address {
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
}

    contract NewStableCoin is Context, IERC20, Ownable {
    using Address for address payable;

    mapping(address => uint256) private _rOwned;
    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;
    mapping(address => bool) private _isExcluded;
    mapping(address => bool) public allowedTransfer;
    mapping(address => bool) private _isBlacklisted;

    address[] private _excluded;

    bool public tradingEnabled=true;
    bool public swapEnabled=true;
    bool private swapping;

    //Anti Dump
    mapping(address => uint256) private _lastTrnx;
    bool public coolDownEnabled = true;
    uint256 public coolDownTime = 60 seconds;

    IRouter public router;
    address public pair;

    uint8 private constant _decimals = 18;
    uint256 private constant MAX = ~uint256(0);

    uint256 private _tTotal = 10_000_000 * 10**_decimals;
    uint256 private _rTotal = (MAX - (MAX % _tTotal));
    uint256 _initalCirtulation =_tTotal * 10 / 100; 
    
          
    uint256 public swapTokensAtAmount = 50000 * 10**_decimals;
    uint256 public maxBuyLimit = 50000 * 10**_decimals;
    uint256 public maxTrnxLimit = 20000 * 10**_decimals;
    uint256 public maxWalletLimit = 50000 * 10**_decimals;
    address public Coin = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
    address public Token;

    uint256 public InitalTokenNumberPerCoin = 100_000_000;
    uint256 public InitalTokenPoolSize = 100_000;
    
    uint256 public ThresoldValue = 50;
    
    uint256 public InitalTokenPoolValue = InitalTokenPoolSize  * 10 ** 18;
    uint256 public InitialCoinPoolValue =  InitalTokenPoolValue / InitalTokenNumberPerCoin;
    
    uint256 public AllowedPriceVariation = InitalTokenNumberPerCoin * ThresoldValue /10000;    
    uint256 public genesis_block = block.number;
    uint256 private deadline;

    address public deadWallet = 0x000000000000000000000000000000000000dEaD;
    string public  constant _name = "STORM BIT GAME";
    string public constant _symbol = "SBGU";

    uint256 public lpFees = 3;

    struct TotFeesPaidStruct {
        uint256 liquidity;
    }

    TotFeesPaidStruct public totFeesPaid;

    struct valuesFromGetValues {
        uint256 rAmount;
        uint256 rTransferAmount;
        uint256 rLiquidity;
        uint256 tTransferAmount;
        uint256 tLiquidity;
    }

    event FeesChanged();
    event UpdatedRouter(address oldRouter, address newRouter);
    
    modifier lockTheSwap() {
        swapping = true;
        _;
        swapping = false;
    }
    constructor(address routerAddress) {
        IRouter _router = IRouter(routerAddress);
        address _pair = IFactory(_router.factory()).createPair(address(this), _router.WETH());

        router = _router;
        pair = _pair;
        Token=address(this);
        _rOwned[owner()] = _rTotal;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[pair] = true;
        _isExcludedFromFee[deadWallet] = true;
        _isExcludedFromFee[0xD152f549545093347A162Dce210e7293f1452150] = true;
        _isExcludedFromFee[0x7ee058420e5937496F5a2096f04caA7721cF70cc] = true;  
     
        allowedTransfer[address(this)] = true;
        allowedTransfer[owner()] = true;
        allowedTransfer[pair] = true;
        allowedTransfer[0xD152f549545093347A162Dce210e7293f1452150] = true;
        allowedTransfer[0x7ee058420e5937496F5a2096f04caA7721cF70cc] = true;
     
        emit Transfer(address(0), owner(), _tTotal);
    }

    //std ERC20:
    function name() public pure returns (string memory) {
        return _name;
    }
    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    //override ERC20:
    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

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

    function transferFrom(address sender,address recipient,uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        _approve(sender, _msgSender(), currentAllowance - amount);
        return true;
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function isExcludedFromReward(address account) public view returns (bool) {
        return _isExcluded[account];
    }

    function reflectionFromToken(uint256 tAmount, bool deductTransferRfi) public view returns (uint256) {
        require(tAmount <= _tTotal, "Amount must be less than supply");
        if (!deductTransferRfi) {
            valuesFromGetValues memory s = _getValues(tAmount, true, false);
            return s.rAmount;
        } else {
            valuesFromGetValues memory s = _getValues(tAmount, true, false);
            return s.rTransferAmount;
        }
    }
 
    function tokenFromReflection(uint256 rAmount) public view returns (uint256) {
        require(rAmount <= _rTotal, "Amount must be less than total reflections");
        uint256 currentRate = _getRate();
        return rAmount / currentRate;
    }

    function isExcludedFromFee(address account) public view returns (bool) {
        return _isExcludedFromFee[account];
    }

    function _takeLiquidity(uint256 rLiquidity, uint256 tLiquidity) private {
        totFeesPaid.liquidity += tLiquidity;
        if (_isExcluded[address(this)]) {
            _tOwned[address(this)] += tLiquidity;
        }
        _rOwned[address(this)] += rLiquidity;
    }

    function _getValues(uint256 tAmount,bool takeFee,bool isSell) private view returns (valuesFromGetValues memory to_return) {
        to_return = _getTValues(tAmount, takeFee, isSell);
        (
            to_return.rAmount,
            to_return.rTransferAmount,
            to_return.rLiquidity
        ) = _getRValues1(to_return, tAmount, takeFee, _getRate());
        return to_return;
    }

    function _getTValues(uint256 tAmount,bool takeFee,bool isSell) private view returns (valuesFromGetValues memory s) {
        if (!takeFee) {
            s.tTransferAmount = tAmount;
            return s;
        }
        uint256 _lpfee;
        if(isSell) _lpfee = lpFees;
        s.tLiquidity = (tAmount * _lpfee) / 100;
        s.tTransferAmount = tAmount - _lpfee;
        return s;
    }

    function _getRValues1(valuesFromGetValues memory s,uint256 tAmount,bool takeFee,uint256 currentRate) private pure
        returns (uint256 rAmount,uint256 rTransferAmount,uint256 rLiquidity)
    {
        rAmount = tAmount * currentRate;
        if (!takeFee) {
            return (rAmount, rAmount, 0);
        }
        rLiquidity = s.tLiquidity * currentRate;
        rTransferAmount = rAmount - rLiquidity ;
        return (rAmount, rTransferAmount, rLiquidity);
    }

    function _getRate() public view returns (uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply / tSupply;
    }

    function _getCurrentSupply() public view returns (uint256, uint256) {
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

    function _approve(address owner,address spender,uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(address from,address to,uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(
            amount <= balanceOf(from),
            "You are trying to transfer more than your balance"
        );
        require(!_isBlacklisted[from] && !_isBlacklisted[to], "You are a bot");

        if (from == pair && !_isExcludedFromFee[to] && !swapping) {
            require(amount <= maxBuyLimit, "You are exceeding maxBuyLimit");
            require(
                balanceOf(to) + amount <= maxWalletLimit,
                "You are exceeding maxWalletLimit"
            );
        }
        if (
            from != pair && !_isExcludedFromFee[to] && !_isExcludedFromFee[from] && !swapping
        ) {
            require(amount <= maxTrnxLimit, "You are exceeding maxSellLimit");
            if (to != pair) {
                require(
                    balanceOf(to) + amount <= maxWalletLimit,
                    "You are exceeding maxWalletLimit"
                );
            }
            if (coolDownEnabled) {
                uint256 timePassed = block.timestamp - _lastTrnx[from];
                require(timePassed >= coolDownTime, "Cooldown enabled");
                _lastTrnx[from] = block.timestamp;
            }
        }
        bool takeFee = true;
        bool isSell = false;
        if (swapping || _isExcludedFromFee[from] || (_isExcludedFromFee[to] && to != pair)) takeFee = false;
        if (to == pair){
            isSell = true;
        } 
        _tokenTransfer(from, to, amount, takeFee, isSell);
    }

    //this method is responsible for taking all fee, if takeFee is true
    function _tokenTransfer(address sender,address recipient,uint256 tAmount,bool takeFee,bool isSell) private {
        valuesFromGetValues memory s = _getValues(tAmount, takeFee, isSell);
        if (_isExcluded[sender]) {           //from excluded
            _tOwned[sender] = _tOwned[sender] - tAmount;
        }
        if (_isExcluded[recipient]) {        //to excluded
            _tOwned[recipient] = _tOwned[recipient] + s.tTransferAmount;
        }
        _rOwned[sender] = _rOwned[sender] - s.rAmount;
        _rOwned[recipient] = _rOwned[recipient] + s.rTransferAmount;

        if (s.rLiquidity > 0 || s.tLiquidity > 0) {
            _takeLiquidity(s.rLiquidity, s.tLiquidity);
            emit Transfer(sender,address(this),s.tLiquidity);
        }
        // if(balanceOf(address(this)) != 0){
        //     AdjustLiquidity();
        // }
        emit Transfer(sender, recipient, s.tTransferAmount);
    }

    function getExchangeRateRate() public view returns(uint256) {
        return IERC20(Token).balanceOf(pair)/IERC20(Coin).balanceOf(pair);
    }

    function getTokenBalance() public view returns(uint256){
        require(Token != address(0), "ERC20: Balance not possible from the zero address");
        uint256 rate = IERC20(Token).balanceOf(pair);
        return rate;
    }

    function getCoinBalance() public view returns(uint256){
        require(Coin != address(0), "ERC20: Balance not possible from the zero address");
        uint256 rate = IERC20(Coin).balanceOf(pair);
        return rate;
    }

    function detectPoolVarition() public view returns (bool,uint256,uint256,uint256){
        require(Coin != address(0), "ERC20 Token: Balance not possible from the zero address");
        require(Token != address(0), "ERC20 Coin: Balance not possible from the zero address");
        uint256 CurrentTokenBalance = IERC20(Token).balanceOf(pair);
        uint256 CurrentCoinBalance = IERC20(Coin).balanceOf(pair);
        uint256 CurrentTokenNumberPerCoin = CurrentTokenBalance/CurrentCoinBalance;
        uint256 CurrentPriceVariation = abs(CurrentTokenNumberPerCoin , InitalTokenNumberPerCoin);
        if(CurrentPriceVariation > AllowedPriceVariation){
            if(CurrentTokenNumberPerCoin > InitalTokenNumberPerCoin){
                uint256 Token_Out =(CurrentPriceVariation * CurrentCoinBalance/2);
                uint256 Coin_in = ((1*10**18)/InitalTokenNumberPerCoin - (1*10**18)/CurrentTokenNumberPerCoin)*CurrentTokenBalance/2/(1*10**18);
                 return (true,Token_Out,Coin_in,1);
            }
            if(CurrentTokenNumberPerCoin < InitalTokenNumberPerCoin){
                uint256 Token_in =(CurrentPriceVariation * CurrentCoinBalance/2);
                uint256 Coin_Out = ((1*10**18)/CurrentTokenNumberPerCoin - (1*10**18)/InitalTokenNumberPerCoin)*CurrentTokenBalance/2/(1*10**18);
                return (true,Token_in,Coin_Out,2);
            }
            else{
                return (false,0,0,0);
            }
        }
        else{
                return (false,0,0,0);
        }
    }
    
    function AdjustLiquidity() public {
        uint256 _Token;
        uint256 coin;
        uint256 dir;
        bool Status;
        (Status,_Token,coin,dir)=detectPoolVarition();
        if(Status){
            if(dir==1){
              swapTokensForBNB(_Token);
            }
            if(dir==2){
              swapEthForToken(coin);
            }
        }
    } 

    function addLiquidity(uint256 tokenAmount, uint256 bnbAmount) private {
        // approve token transfer to cover all possible scenarios
        _approve(address(this), address(router), tokenAmount);
        // add the liquidity
        (bool success, ) = address(router).call{value: bnbAmount}(
            abi.encodeWithSignature("addLiquidityETH(address,uint256,uint256,uint256,address,uint256)", address(this), tokenAmount, 0, 0, address(this), block.timestamp));
        require(success, "Add liquidity failed");
    }

    function swapTokensForBNB(uint256 tokenAmount) private {
        // generate the pair path of token -> weth
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();

        _approve(address(this), address(router), tokenAmount);
        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of ETH
            path,
            address(this),
            block.timestamp
        );
    }

    function swapEthForToken(uint256 _BNBAmount) private {
        // generate the pair path of token -> weth
        address[] memory path = new address[](2);
        path[0] = router.WETH();
        path[1] = address(this);

        _approve(address(this), address(router), _BNBAmount);
        router.swapExactETHForTokensSupportingFeeOnTransferTokens(
            _BNBAmount,
            path,
            address(this),
            block.timestamp
        );
    }

    function abs(uint256  x, uint256 y) private pure returns (uint256) {
        if(x>=y)
        {
           return (x-y);
        }
        else
        {
           return(y-x);
        }
    }

    function setLiquidityFeePercent(uint256 _newlpFee) external onlyOwner {
        lpFees = _newlpFee;
    }

    function setThresoldValue(uint256 _newThresoldValue) external onlyOwner{
        ThresoldValue = _newThresoldValue;
    }

    function updateRouterAndPair(address newRouter, address newPair) external onlyOwner {
        require(newRouter != address(0),"New router address can not be zero");
        router = IRouter(newRouter);
        require(newPair != address(0),"New pair address can not be zero");
        pair = newPair;
        _isExcludedFromFee[pair] = true;
        allowedTransfer[pair] = true;
    }

    function rescueBNB(uint256 weiAmount) external onlyOwner {
        require(address(this).balance >= weiAmount, "insufficient BNB balance");
        payable(msg.sender).transfer(weiAmount);
    }

    function rescueAnyBEP20Tokens(address _tokenAddr,address _to,uint256 _amount) public onlyOwner {
        require(_tokenAddr != address(0), "tokenAddress cannot be the zero address");
        require(_to != address(0), "receiver cannot be the zero address");
        require(_amount > 0, "amount should be greater than zero");
        IERC20 token = IERC20(_tokenAddr);
        bool success = token.transfer(_to, _amount);
        require(success, "Token transfer failed");
    }
    receive() external payable {}
}