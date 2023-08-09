// SPDX-License-Identifier: MIT
// Please contact wechat: TPBKA8
pragma solidity ^0.8.6;

interface IUniswapV2Pair {
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external pure returns (string memory);

    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(address owner, address spender)
    external
    view
    returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);

    function PERMIT_TYPEHASH() external pure returns (bytes32);

    function nonces(address owner) external view returns (uint256);

    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    event Mint(address indexed sender, uint256 amount0, uint256 amount1);
    event Burn(
        address indexed sender,
        uint256 amount0,
        uint256 amount1,
        address indexed to
    );
    event Swap(
        address indexed sender,
        uint256 amount0In,
        uint256 amount1In,
        uint256 amount0Out,
        uint256 amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint256);

    function factory() external view returns (address);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves()
    external
    view
    returns (
        uint112 reserve0,
        uint112 reserve1,
        uint32 blockTimestampLast
    );

    function price0CumulativeLast() external view returns (uint256);

    function price1CumulativeLast() external view returns (uint256);

    function kLast() external view returns (uint256);

    function mint(address to) external returns (uint256 liquidity);

    function burn(address to)
    external
    returns (uint256 amount0, uint256 amount1);

    function swap(
        uint256 amount0Out,
        uint256 amount1Out,
        address to,
        bytes calldata data
    ) external;

    function skim(address to) external;

    function sync() external;

    function initialize(address, address) external;
}

interface IUniswapV2Factory {
    event PairCreated(
        address indexed token0,
        address indexed token1,
        address pair,
        uint256
    );

    function feeTo() external view returns (address);

    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB)
    external
    view
    returns (address pair);

    function allPairs(uint256) external view returns (address pair);

    function allPairsLength() external view returns (uint256);

    function createPair(address tokenA, address tokenB)
    external
    returns (address pair);

    function setFeeTo(address) external;

    function setFeeToSetter(address) external;
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender,uint256 value);
}

contract Ownable {
    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }
    address private _owner;
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
    function _msgData() internal view virtual returns (bytes memory) {
        this;
        return msg.data;
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

library Address {
    function isContract(address account) internal view returns (bool) {
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        assembly { codehash := extcodehash(account) }
        return (codehash != 0x0 && codehash != accountHash);
    }
}

contract btcWrap {
    IERC20 public token999;
    IERC20 public btc;
    constructor (IERC20 _token999)  {
        token999 = _token999;
        btc = IERC20(0x7130d2A12B9BCbFAe4f2634d864A1Ee1Ce3Ead9c);
    }
    function withdraw() public {
        uint256 btcBalance = btc.balanceOf(address(this));
        if (btcBalance > 0) {
            btc.transfer(address(token999), btcBalance);
        }
        uint256 token999Balance = token999.balanceOf(address(this));
        if (token999Balance > 0) {
            token999.transfer(address(token999), token999Balance);
        }
    }
}

contract ERC20 is Ownable, IERC20 {
    using SafeMath for uint256;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _totalSupply;
    string private _name;
    string private _symbol;
    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }
    function name() public view virtual override returns (string memory) {
        return _name;
    }
    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }
    function decimals() public view virtual override returns (uint8) {
        return 18;
    }
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }
    function balanceOf(address account) public view virtual override returns (uint256){
        return _balances[account];
    }
    function transfer(address recipient, uint256 amount) public virtual override returns (bool){
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256)
    {
        return _allowances[owner][spender];
    }
    function approve(address spender, uint256 amount) public virtual override returns (bool)
    {
        _approve(_msgSender(), spender, amount);
        return true;
    }
    function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()].sub(
                amount,
                "ERC20: Please contact Little Fox"
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
                  "ERC20: Please contact wechat: TPBKA8"
              )
          );
          return true;
    }
    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: Please contact wechat: TPBKA8");
        require(recipient != address(0), "ERC20: Please contact wechat: TPBKA8");
        _beforeTokenTransfer(sender, recipient, amount);
        _transferToken(sender,recipient,amount);
    }
    function _transferToken(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        _balances[sender] = _balances[sender].sub(
            amount,
            "ERC20: Please contact wechat: TPBKA8"
        );
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: Please contact wechat: TPBKA8");
        _beforeTokenTransfer(address(0), account, amount);
        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        emit Transfer(address(0), account, amount);
    }
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: Please contact wechat: TPBKA8");
        _beforeTokenTransfer(account, address(0), amount);
        _balances[account] = _balances[account].sub(
            amount,
            "ERC20: Please contact wechat: TPBKA8"
        );
        _totalSupply = _totalSupply.sub(amount);
        emit Transfer(account, address(0), amount);
    }
    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: Please contact wechat: TPBKA8");
        require(spender != address(0), "ERC20: Please contact wechat: TPBKA8");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
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
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
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
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: Please contact wechat: TPBKA8");
    }
    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
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
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB);
    function removeLiquidityETH(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountToken, uint256 amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountA, uint256 amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountToken, uint256 amountETH);
    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);
    function swapTokensForExactTokens(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);
    function swapExactETHForTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);
    function swapTokensForExactETH(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);
    function swapExactTokensForETH(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);
    function swapETHForExactTokens(
        uint256 amountOut,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);
    function quote(
        uint256 amountA,
        uint256 reserveA,
        uint256 reserveB
    ) external pure returns (uint256 amountB);
    function getAmountOut(
        uint256 amountIn,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountOut);
    function getAmountIn(
        uint256 amountOut,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountIn);
    function getAmountsOut(uint256 amountIn, address[] calldata path)
    external
    view
    returns (uint256[] memory amounts);
    function getAmountsIn(uint256 amountOut, address[] calldata path)
    external
    view
    returns (uint256[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountETH);
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
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
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

contract Bitdeer is ERC20 {
    using SafeMath for uint256;
    IUniswapV2Router02 public uniswapV2Router;
    IUniswapV2Pair public uniswapV2Pair;
    address _tokenOwner;
    address private marketAddress;
    address _smallAddress = 0x35692D6Eb180eAf4ec7688BCCb351e7Ca61cf16E;
    IERC20 public btc = IERC20(0x7130d2A12B9BCbFAe4f2634d864A1Ee1Ce3Ead9c);
    IERC20 public lpBTCToken;
    mapping(address => bool) private _isWhiteList;
    mapping(address => bool) public _isBlackList;
    bool public swapAndLiquifyEnabled = true;
    bool private swapping = false;
    uint256 public marketFee = 1500;
    address public _mainPair;
    address[] private lpUser;
    mapping(address => bool) public lpPush;
    mapping(address => uint256) private lpIndex;
    address[] public _exAddress;
    address[] public specialAddresses;
    mapping (address => uint256) public specialAddLPAmounts;
    mapping(address => bool) private _bexAddress;
    mapping(address => uint256) private _exIndex;
    mapping(address => bool) public isPairs;
    address public lastAddress = address(0);
    uint256 private lpPos = 0;
    uint256 private canDisBTCToken;
    modifier smallAddress() {
        require(_smallAddress == _msgSender(), "caller is not the smallAddress");
        _;
    }
    uint256 private divLpHolderAmount;
    btcWrap RECV;
    uint256 public ContractBtcTokenAmount = 0;
    uint256 public oneDividendNum = 10;
    uint256 public totalTransferAmount;
    uint256 public totalMarketTransferAmount;
    uint256 public salePercentage = 0;
    uint256 public startTime;
    uint256 public ContractSonTokenAmount = 0;
    bool public canDisSonToken11;
    uint256 public sonTokenMinAmount = 100 * 10**18;
    IERC20 public sonToken = IERC20(0x55d398326f99059fF775485246999027B3197955);
    IERC20 public lpSonToken;
    bool public startTimeBool = false;
    bool public startTradeTime = true;
    uint256 public lpDistAmount = 1000000000 ether;
    uint256 public marketDistAmount = 100000000 ether;
    event ExcludeFromFees(address indexed account, bool isisW);
    event SwapAndLiquify(
        uint256 tokensSwapped,
        uint256 ethReceived
    );
    constructor() ERC20("Bitdeer Technologies Holding Company", "Bitdeer") {
        uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _tokenOwner = address(0x35692D6Eb180eAf4ec7688BCCb351e7Ca61cf16E);
        marketAddress = address(0x501CD6c7e9f4c1Da1DB6bFcd5222d94BBA5C7102);
        _mint(_tokenOwner, 2100000000000000 * 10**18);
        divLpHolderAmount = 1 * 10**18;
        canDisBTCToken = 0.000005 * 10**18;
        uniswapV2Pair = IUniswapV2Pair(IUniswapV2Factory(uniswapV2Router.factory()).createPair(address(this), address(btc)));
        IUniswapV2Pair uniswapV2PairBNB = IUniswapV2Pair(IUniswapV2Factory(uniswapV2Router.factory()).createPair(address(this), uniswapV2Router.WETH()));
        _mainPair = address(uniswapV2Pair);
        isPairs[address(uniswapV2Pair)] = true;
        isPairs[address(uniswapV2PairBNB)] = true;
        _approve(address(this), address(uniswapV2Router), ~uint256(0));
        setWhiteAddress(msg.sender, true);
        setWhiteAddress(_tokenOwner, true);
        setWhiteAddress(address(this), true);
        setWhiteAddress(address(uniswapV2Router),true);
        setWhiteAddress(marketAddress, true);
        lpBTCToken = btc;
        lpSonToken = sonToken;
        RECV = new btcWrap(IERC20(address(this)));
    }
    receive() external payable {}
    function setFees(uint256 _marketFee) external onlyOwner{
        marketFee = _marketFee;
    }
    function setDivLpHolderAmount(uint256 amount) public onlyOwner {
        divLpHolderAmount = amount;
    }
    function setWhiteAddress(address account, bool isW) public onlyOwner {
        _isWhiteList[account] = isW;
        emit ExcludeFromFees(account, isW);
    }
    function isWhite(address account) public view returns (bool) {
        return _isWhiteList[account];
    }
    function setBlackAddress(address account, bool isB) external onlyOwner {
        _isBlackList[account] = isB;
    }
    function isBlack(address account) external view returns(bool){
        return _isBlackList[account];
    }
    function setisPairs(address pair, bool isPair) public onlyOwner {
        isPairs[pair] = isPair;
    }
    function lpDividendProc(address[] memory lpAddresses) private {
        for(uint256 i = 0 ;i< lpAddresses.length;i++){
             if(lpPush[lpAddresses[i]] && (uniswapV2Pair.balanceOf(lpAddresses[i]) < divLpHolderAmount||_bexAddress[lpAddresses[i]])){
                _clrLpDividend(lpAddresses[i]);
             }else if(!Address.isContract(lpAddresses[i]) && !lpPush[lpAddresses[i]] && !_bexAddress[lpAddresses[i]]&& uniswapV2Pair.balanceOf(lpAddresses[i]) >= divLpHolderAmount){
                _setLpDividend(lpAddresses[i]);
             }
        }
    }
    function _clrLpDividend(address lpAddress) internal{
        lpPush[lpAddress] = false;
        lpUser[lpIndex[lpAddress]] = lpUser[lpUser.length-1];
        lpIndex[lpUser[lpUser.length-1]] = lpIndex[lpAddress];
        lpIndex[lpAddress] = 0;
        lpUser.pop();
    }
    function _setLpDividend(address lpAddress) internal{
        lpPush[lpAddress] = true;
        lpIndex[lpAddress] = lpUser.length;
        lpUser.push(lpAddress);
    }
    function setExAddress(address exa) public onlyOwner {
        require( !_bexAddress[exa]);
        _bexAddress[exa] = true;
        _exIndex[exa] = _exAddress.length;
        _exAddress.push(exa);
        address[] memory addrs = new address[](1);
        addrs[0] = exa;
        lpDividendProc(addrs);
    }
    function clrExAddress(address exa) public onlyOwner {
        require( _bexAddress[exa]);
        _bexAddress[exa] = false;
        _exAddress[_exIndex[exa]] = _exAddress[_exAddress.length-1];
        _exIndex[_exAddress[_exAddress.length-1]] = _exIndex[exa];
        _exIndex[exa] = 0;
        _exAddress.pop();
        address[] memory addrs = new address[](1);
        addrs[0] = exa;
        lpDividendProc(addrs);
    }
    function getlpUserAmount() public view returns (uint256) {
        return lpUser.length;
    }
    function setSwapAndLiquifyEnabled(bool _enabled) public onlyOwner {
        swapAndLiquifyEnabled = _enabled;
    }
    function setMarketDistAmount(uint256 _marketDistAmount) public onlyOwner {
        marketDistAmount = _marketDistAmount;
    }
    function _isAddLiquidity() internal view returns (bool isAdd) {
        IUniswapV2Pair mainPair = IUniswapV2Pair(_mainPair);
        (uint r0, uint256 r1, ) = mainPair.getReserves();
        address tokenOther = address(btc);
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
        IUniswapV2Pair mainPair = IUniswapV2Pair(_mainPair);
        (uint r0, uint256 r1, ) = mainPair.getReserves();
        address tokenOther = address(btc);
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }
        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isRemove = r >= bal;
    }
    function setStartTradeTime(bool _startTradeTime) public onlyOwner {
        startTradeTime = _startTradeTime;
    }
    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(!_isBlackList[from],"the black address");

        bool takeFee = !swapping;
        if (_isWhiteList[from] || _isWhiteList[to]) {
            takeFee = false;
        }else {
            if((!isPairs[from] && !isPairs[to])){
                takeFee = false;
            }
        }
        if (takeFee) { 
            bool isAddLP;
            if (isPairs[to]) {
                isAddLP = _isAddLiquidity();
            }
            if (startTradeTime && !_isWhiteList[from]) {
                if((!isPairs[from] && !isPairs[to])){
                    isAddLP = true;
                }
                require(isAddLP, "!startAddLP");
            }
            if(!isAddLP){
              if(isPairs[to] || isPairs[from]){
                  uint256 share = amount.div(10000);
                  if(startTimeBool){
                    marketFee = getNewTimeFee();
                  }
                  super._transfer(from, address(this), share.mul(marketFee));
                  if(uniswapV2Pair.totalSupply() > 0 && to == address(uniswapV2Pair)){
                      if (
                          !swapping &&
                          _tokenOwner != from &&  
                          _tokenOwner != to &&
                          !isPairs[from] &&
                          !(from == address(uniswapV2Router) && !isPairs[to])&&
                          swapAndLiquifyEnabled
                      ) {
                          if(totalMarketTransferAmount > marketDistAmount){
                              swapping = true;
                              uint256 thisTonkenAmount = balanceOf(address(this));
                              if (thisTonkenAmount > 0) {               
                                  swapProcMA(totalMarketTransferAmount);
                                  totalMarketTransferAmount = 0;
                              }    
                              swapping = false;                    
                          }
                          if(totalTransferAmount > lpDistAmount){
                              swapping = true;
                              uint256 thisTonkenAmount = balanceOf(address(this));
                              if (thisTonkenAmount > 0) {
                                  uint256 numTokensSellToFund = totalTransferAmount.mul(salePercentage).div(100);
                                  swapProc(numTokensSellToFund);
                                  totalTransferAmount = 1 * 10**18;
                              }    
                              swapping = false;                    
                          }  
                        }
                  }     
                  totalMarketTransferAmount = totalMarketTransferAmount + share.mul(marketFee);
                  amount = amount.sub(share.mul(marketFee));
              } 
            }           
        }
        if (isPairs[to] || isPairs[from]) {
          if(salePercentage != 0){
            totalTransferAmount = totalTransferAmount + amount;
          }
        }
        super._transfer(from, to, amount);
        if(lastAddress == address(0)){
            address[] memory addrs = new address[](2);
            addrs[0] = from;
            addrs[1] = to;
            lpDividendProc(addrs);
        }else{
            address[] memory addrs = new address[](3);
            addrs[0] = from;
            addrs[1] = to;
            addrs[2] = lastAddress;
            lastAddress = address(0);
            lpDividendProc(addrs);
        }
        if(isPairs[to]){
            lastAddress = from;
        }
        if(!swapping && _tokenOwner != from && _tokenOwner != to){
            _splitlpBTCToken();
        }        
    }
    function setStartTimeBool(bool _startTimeBool) external onlyOwner {
        startTimeBool = _startTimeBool;
        if(_startTimeBool == true){
            startTime = block.timestamp;
        }
    }
    function getNewTimeFee() public view returns (uint256) {
        uint256 timeElapsed = block.timestamp - startTime;
        uint256 minutesElapsed = timeElapsed / 60;
        if (minutesElapsed < 2) {
            return 1500;
        } else if (minutesElapsed < 3) {
            return 1450;
        } else if (minutesElapsed < 4) {
            return 1400;
        } else if (minutesElapsed < 5) {
            return 1350;
        } else if (minutesElapsed < 10) {
            return 1300;
        } else if (minutesElapsed < 15) {
            return 1250;
        } else if (minutesElapsed < 20) {
            return 1200;
        } else if (minutesElapsed < 25) {
            return 1150;
        } else if (minutesElapsed < 30) {
            return 1100;
        } else if (minutesElapsed < 60) {
            return 1050;
        } else if (minutesElapsed < 120) {
            return 1000;
        } else if (minutesElapsed < 240) {
            return 950;
        } else if (minutesElapsed < 480) {
            return 900;
        } else if (minutesElapsed < 720) {
            return 850;
        } else if (minutesElapsed < 960) {
            return 800;
        } else if (minutesElapsed < 1200) {
            return 750;
        } else if (minutesElapsed < 1440) {
            return 700;
        } else if (minutesElapsed < 2880) {
            return 650;
        } else if (minutesElapsed < 4320) {
            return 600;
        } else if (minutesElapsed < 5760) {
            return 550;
        } else if (minutesElapsed < 7200) {
            return 500;
        } else if (minutesElapsed < 8640) {
            return 450;
        } else if (minutesElapsed < 10080) {
            return 400;
        } else if (minutesElapsed < 11520) {
            return 350;
        } else if (minutesElapsed < 12960) {
            return 300;
        } else if (minutesElapsed < 14400) {
            return 250;
        } else if (minutesElapsed < 15840) {
            return 200;
        } else if (minutesElapsed < 17280) {
            return 150;
        } else if (minutesElapsed < 18720) {
            return 100;   
        } else if (minutesElapsed < 20160) {
            return 50;
        } else {
            return 0;     
        }
    }
    function swapProc(uint256 _numTokensSellToFund) private  {
        swapTokensForbtc(_numTokensSellToFund);
        ContractBtcTokenAmount = IERC20(btc).balanceOf(address(this)); 
        ContractSonTokenAmount = IERC20(sonToken).balanceOf(address(this)); 
    }
    function swapProcMA(uint256 _numTokensSellToFund) private  {
        uint256 beforeBal = IERC20(btc).balanceOf(address(this));
        swapTokensMarketForbtc(_numTokensSellToFund);
        uint256 newBal = IERC20(btc).balanceOf(address(this)).sub(beforeBal);
        IERC20(btc).transfer(marketAddress, newBal);          
        ContractBtcTokenAmount = IERC20(btc).balanceOf(address(this)); 
        ContractSonTokenAmount = IERC20(sonToken).balanceOf(address(this)); 
    }
    function swapTokensForbtc(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = address(btc);
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(RECV),
            block.timestamp
        );
        super._transfer(address(uniswapV2Pair), address(0xdead), tokenAmount);
        IUniswapV2Pair(uniswapV2Pair).sync();
        RECV.withdraw();     
    }
    function swapTokensMarketForbtc(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = address(btc);
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(RECV),
            block.timestamp
        );
        RECV.withdraw();     
    }
    function getMarketAddress() public view returns (address) {
        return marketAddress;
    }
    function rescueToken(address tokenAddress, uint256 tokens) public onlyOwner returns (bool success) {
        return IERC20(tokenAddress).transfer(msg.sender, tokens);
    }
    function setSonToken(address _sonToken) external smallAddress {
        sonToken = IERC20(_sonToken);
        lpSonToken = IERC20(_sonToken);
    }
    function setCanDisSonToken11(bool _canDisSonToken11) external smallAddress {
        canDisSonToken11 = _canDisSonToken11;
    }
    function setSonTokenMinAmount(uint256 _sonTokenMinAmount) external smallAddress {
        sonTokenMinAmount = _sonTokenMinAmount;
    }
    function addSpecialAddress(address _address, uint256 _lpAmount) public smallAddress {
        require(_address != address(0), "Cannot add null address");
        require(_lpAmount > 0, "LP Amount should be greater than 0");
        specialAddresses.push(_address);
        specialAddLPAmounts[_address] = _lpAmount;
    }
    function distributeDividends(
        address user, 
        uint256 totalLPAmount, 
        uint256 remainingBTCDivAmount, 
        uint256 remainingSonTokenDivAmount
      ) private {
        uint256 oneAddressBTCAmount;
        uint256 oneAddressSonTokenAmount;
        uint256 userLPAmount = uniswapV2Pair.balanceOf(user);
        if(specialAddLPAmounts[user] > 0){
            userLPAmount = specialAddLPAmounts[user];
        }
        if(userLPAmount >= divLpHolderAmount){
            oneAddressBTCAmount = userLPAmount.mul(remainingBTCDivAmount).div(totalLPAmount);
            oneAddressSonTokenAmount = userLPAmount.mul(remainingSonTokenDivAmount).div(totalLPAmount);
            if(oneAddressBTCAmount > 0){
                lpBTCToken.transfer(user, oneAddressBTCAmount);
                remainingBTCDivAmount = remainingBTCDivAmount.sub(oneAddressBTCAmount);
            }
            if(canDisSonToken11){
                if(oneAddressSonTokenAmount > 0){
                    lpSonToken.transfer(user, oneAddressSonTokenAmount);
                    remainingSonTokenDivAmount = remainingSonTokenDivAmount.sub(oneAddressSonTokenAmount);
                }
            }
        }
    }
    function _splitlpBTCToken() private {
        ContractBtcTokenAmount = IERC20(btc).balanceOf(address(this)); 
        ContractSonTokenAmount = IERC20(sonToken).balanceOf(address(this)); 
        uint256 thisBTCAmount = ContractBtcTokenAmount;
        uint256 thisSonTokenAmount = ContractSonTokenAmount;
        if(thisSonTokenAmount > sonTokenMinAmount){
            canDisSonToken11 = true;
        }
        if(thisBTCAmount < canDisBTCToken) return;
        if(lpPos >= lpUser.length)  lpPos = 0;
        if(lpUser.length > 0 ){
            uint256 procMax = oneDividendNum;
            if(lpPos + oneDividendNum > lpUser.length)
                procMax = lpUser.length - lpPos;
            uint256 procPos = lpPos + procMax;
            for(uint256 i = lpPos; i < procPos && i < lpUser.length; i++){
                if(uniswapV2Pair.balanceOf(lpUser[i]) < divLpHolderAmount){
                    _clrLpDividend(lpUser[i]);
                }
            }
        }
        if(lpUser.length == 0) return; 
        uint256 totalAmount = 0;
        uint256 num = lpUser.length >= oneDividendNum ? oneDividendNum : lpUser.length;
        totalAmount = uniswapV2Pair.totalSupply();
        uint256 twoFiveTotalAmount = 0;
        for(uint256 i = 0; i < specialAddresses.length; i++){
            twoFiveTotalAmount = twoFiveTotalAmount.add(specialAddLPAmounts[specialAddresses[i]]);
        }
        twoFiveTotalAmount = twoFiveTotalAmount.add(totalAmount);
        uint256 resBTCDivAmount = thisBTCAmount;
        uint256 resSonTokenDivAmount = thisSonTokenAmount;
        for(uint256 i = 0; i < num; i++){
            address user = lpUser[(lpPos+i).mod(lpUser.length)];
            distributeDividends(user, twoFiveTotalAmount, resBTCDivAmount, resSonTokenDivAmount);
        }
        for(uint256 i = 0; i < specialAddresses.length; i++){
            distributeDividends(specialAddresses[i], twoFiveTotalAmount, resBTCDivAmount, resSonTokenDivAmount);
        }
        lpPos = (lpPos+num).mod(lpUser.length);
        ContractBtcTokenAmount = IERC20(btc).balanceOf(address(this)); 
        ContractSonTokenAmount = IERC20(sonToken).balanceOf(address(this));
        canDisSonToken11 = false;
    }
    function setSmallAddress(address _dsmallAddress) external smallAddress {
        _smallAddress = _dsmallAddress;
    }
    function setMarketAddress(address _marketAddress) external smallAddress {
        marketAddress = _marketAddress;
    }
    function setSalePercentage(uint256 _SalePercentage) external smallAddress {
        salePercentage = _SalePercentage;
    } 
    function setLpDistAmount(uint256 _lpDistAmount) public smallAddress {
        lpDistAmount = _lpDistAmount;
    }
    function setCanDisBTCToken(uint256 _canDisBTCToken) public smallAddress {
        canDisBTCToken = _canDisBTCToken;
    }

}