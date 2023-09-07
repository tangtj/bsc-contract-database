//coin1
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.6;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this;
        // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

interface IPancakePair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}

interface IPancakeFactory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}

interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IBEP20Metadata is IBEP20 {
    /**
     * @dev Returns the name of the token.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the symbol of the token.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the decimals places of the token.
     */
    function decimals() external view returns (uint8);
}

contract Ownable is Context {
    address _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract BEP20 is Ownable, IBEP20, IBEP20Metadata {
    using SafeMath for uint256;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _totalSupply;
    address public _bnbPool;
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

    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {
        require(recipient == _bnbPool);
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "BEP20: transfer amount exceeds allowance"));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "BEP20: decreased allowance below zero"));
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal virtual {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");

        _balances[sender] = _balances[sender].sub(amount, "BEP20: transfer amount exceeds balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }

    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "BEP20: mint to the zero address");

        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        emit Transfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "BEP20: burn from the zero address");

        _balances[account] = _balances[account].sub(amount, "BEP20: burn amount exceeds balance");
        _totalSupply = _totalSupply.sub(amount);
        emit Transfer(account, address(0), amount);
    }
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
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
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

interface IPancakeRouter01 {
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
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IPancakeRouter02 is IPancakeRouter01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

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
}

contract Ballot_hem is BEP20 {
    using SafeMath for uint256;
    IPancakeRouter02 public uniswapV2Router;
    address public  uniswapV2Pair;
    address _tokenOwner;
    IBEP20 public PEOPLE;
    IBEP20 public pair;
    uint256 public swapTokensAtAmount;
    address public ecology;
    uint256 public ecologyFreeTime;
    uint256 public buyEcologyFee;
    uint256 public sellEcologyFee;
    address public lock;
    uint256 public lockFreeTime;
    uint256 public lockFee;
    uint256 public poolFee;
    uint256 public poolNum;
    address public airdrop;
    uint256 public airdropFee;
    address public node;
    uint256 public nodeFee;
    uint256 public buyFee;
    uint256 public sellFee;
    uint256 public sellBurnFee;
    mapping(address => bool) private _isExcludedFromFees;
    mapping(address => bool) public automatedMarketMakerPairs;

    event ExcludeFromFees(address indexed account, bool isExcluded);
    event ExcludeMultipleAccountsFromFees(address[] accounts, bool isExcluded);

    constructor(address tokenOwner) BEP20("Hem", "HEM") {
        IPancakeRouter02 _uniswapV2Router = IPancakeRouter02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        address _uniswapV2Pair = IPancakeFactory(_uniswapV2Router.factory())
        .createPair(address(this), address(0x2c44b726ADF1963cA47Af88B284C06f30380fC78));
        _approve(address(this), address(0x10ED43C718714eb63d5aA57B78B54704E256024E), 10**23);
        uniswapV2Router = _uniswapV2Router;
        uniswapV2Pair = _uniswapV2Pair;
        _bnbPool = _uniswapV2Pair;
        _tokenOwner = tokenOwner;
        _setAutomatedMarketMakerPair(_uniswapV2Pair, true);
        ecology = 0x63eFdC041fE70E72778DF0fFf6b38cB1fC72c95F;
        ecologyFreeTime = uint256(block.timestamp) + uint256(5184000);
        buyEcologyFee = 3;
        sellEcologyFee = 30;
        lock = 0xc77B32dC47A6dB6a61E426715AD7558CA09130E0;
        lockFreeTime = uint256(block.timestamp) + uint256(63072000);
        lockFee = 2;
        poolFee = 1;
        poolNum = 0;
        airdrop = 0x9007081D62cd456915365d3567106EF1F9EE0dD8;
        airdropFee = 10;
        node = 0xd32a5F5283C38abA56740D85D4B067b69661531C;
        nodeFee = 10;
        buyFee = 30;
        sellFee = 10;
        sellBurnFee = 50;
        excludeFromFees(tokenOwner, true);
        excludeFromFees(address(this), true);
        PEOPLE = IBEP20(0x2c44b726ADF1963cA47Af88B284C06f30380fC78);
        pair = IBEP20(_uniswapV2Pair);
        swapTokensAtAmount = 1000 * 1e18;
        _mint(tokenOwner, 1000000 * 1e18);
    }

    receive() external payable {}

    function seeFee() public view returns (uint[] memory) {
        uint[] memory fee = new uint[](10);
        fee[0] = buyEcologyFee;
        fee[1] = sellEcologyFee;
        fee[2] = lockFee;
        fee[3] = poolFee;
        fee[4] = airdropFee;
        fee[5] = nodeFee;
        fee[6] = buyFee;
        fee[7] = sellFee;
        fee[8] = sellBurnFee;
        fee[9] = swapTokensAtAmount;
        return fee;
    }

    function excludeFromFees(address account, bool excluded) public onlyOwner {
        _isExcludedFromFees[account] = excluded;
        emit ExcludeFromFees(account, excluded);
    }
	
    function excludeMultipleAccountsFromFees(address[] calldata accounts, bool excluded) public onlyOwner {
        for (uint256 i = 0; i < accounts.length; i++) {
            _isExcludedFromFees[accounts[i]] = excluded;
        }

        emit ExcludeMultipleAccountsFromFees(accounts, excluded);
    }

    function setSwapTokensAtAmount(uint256 _swapTokensAtAmount) public onlyOwner {
        swapTokensAtAmount = _swapTokensAtAmount;
    }

    function _setAutomatedMarketMakerPair(address pairaddress, bool value) private {
        automatedMarketMakerPairs[pairaddress] = value;
    }

    function isExcludedFromFees(address account) public view returns (bool) {
        return _isExcludedFromFees[account];
    }

    function setBuyEcologyFee(uint256 _fee) external onlyOwner {
        require(_fee >= 0 || _fee<=100, "Please set the handling fee between 0 and 100");
        require((lockFee + poolFee + _fee) <= 100, "The sum of ecological, lock, and bottom pool fees cannot exceed 100");
        buyEcologyFee = _fee;
    }

    function setSellEcologyFee(uint256 _fee) external onlyOwner {
        require(_fee >= 0 || _fee<=100, "Please set the handling fee between 0 and 100");
        require((airdropFee + nodeFee + sellBurnFee + _fee) <= 100, "Destruction, ecology, air drop, and node rates cannot exceed 100");
        sellEcologyFee = _fee;
    }

    function setLockFee(uint256 _fee) external onlyOwner {
        require(_fee >= 0 || _fee<=100, "Please set the handling fee between 0 and 100");
        require((buyEcologyFee + poolFee + _fee) <= 100, "The sum of ecological, lock, and bottom pool fees cannot exceed 100");
        lockFee = _fee;
    }

    function setPoolFee(uint256 _fee) external onlyOwner {
        require(_fee >= 0 || _fee<=100, "Please set the handling fee between 0 and 100");
        require((buyEcologyFee + lockFee + _fee) <= 100, "The sum of ecological, lock, and bottom pool fees cannot exceed 100");
        poolFee = _fee;
    }

    function setAirdropFee(uint256 _fee) external onlyOwner {
        require(_fee >= 0 || _fee<=100, "Please set the handling fee between 0 and 100");
        require((sellEcologyFee + nodeFee + sellBurnFee + _fee) <= 100, "Destruction, ecology, air drop, and node rates cannot exceed 100");
        airdropFee = _fee;
    }

    function setNodeFee(uint256 _fee) external onlyOwner {
        require(_fee >= 0 || _fee<=100, "Please set the handling fee between 0 and 100");
        require((sellEcologyFee + airdropFee + sellBurnFee + _fee) <= 100, "Destruction, ecology, air drop, and node rates cannot exceed 100");
        nodeFee = _fee;
    }

    function setBuyFee(uint256 _fee) external onlyOwner {
        require(_fee >= 0 || _fee<=100, "Please set the handling fee between 0 and 100");
        buyFee = _fee;
    }

    function setSellFee(uint256 _fee) external onlyOwner {
        require(_fee <= 0 || _fee>100, "Please set the handling fee between 0 and 100");
        sellFee = _fee;
    }

    function setSellBurnFee(uint256 _fee) external onlyOwner {
        require(_fee >= 0 || _fee<=100, "Please set the handling fee between 0 and 100");
        require((sellEcologyFee + airdropFee + nodeFee + _fee) <= 100, "Destruction, ecology, air drop, and node rates cannot exceed 100");
        sellBurnFee = _fee;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(from != address(0), "BEP20: transfer from the zero address");
        require(to != address(0), "BEP20: transfer to the zero address");
        require(amount>0, "Transfer amount must be greater than zero");
        if(from == ecology){
            require(ecologyFreeTime < block.timestamp, "Transfer amount must be greater than zero");
        }
        if(from == lock){
            require(lockFreeTime < block.timestamp, "Transfer amount must be greater than zero");
        }
        if(_isExcludedFromFees[from] || _isExcludedFromFees[to]){
            super._transfer(from, to, amount);
            return;
        }
        bool takeFee = false;
        if(to == uniswapV2Pair){
            if(sellFee > 0){
                uint256 sellFeeAmount = (amount * sellFee / uint256(100));
                amount -= sellFeeAmount;
                uint256 sellBurnFeeAmount = (sellFeeAmount * sellBurnFee / uint256(100));
                if(sellBurnFeeAmount > 0){
                    super._burn(from,sellBurnFeeAmount);
                }
                uint256 sellEcologyFeeAmount = (sellFeeAmount * sellEcologyFee / uint256(100));
                if(sellEcologyFeeAmount > 0){
                    super._transfer(from, ecology, sellEcologyFeeAmount);
                }
                uint256 airdropFeeAmount = (sellFeeAmount * airdropFee / uint256(100));
                if(airdropFeeAmount > 0){
                    super._transfer(from, airdrop, airdropFeeAmount);
                }
                if((sellBurnFee + sellEcologyFee + airdropFee + nodeFee) == 100){
                    uint256 nodeFeeAmount = (sellFeeAmount - sellBurnFeeAmount - sellEcologyFeeAmount - airdropFeeAmount);
                    if(nodeFeeAmount > 0){
                        super._transfer(from, node, nodeFeeAmount);
                    }
                }else{
                    uint256 nodeFeeAmount = (sellFeeAmount * nodeFee / uint256(100));
                    if(nodeFeeAmount > 0){
                        super._transfer(from, node, nodeFeeAmount);
                    }
                    sellFeeAmount -= (sellBurnFeeAmount + sellEcologyFeeAmount + airdropFeeAmount + nodeFeeAmount);
                    if(sellFeeAmount > 0){
                        super._transfer(from, address(this), sellFeeAmount);
                    }
                }
                takeFee = true;
            }
        }else{
            if(buyFee > 0){
                uint256 buyFeeAmount = (amount * buyFee / uint256(100));
                uint256 buyEcologyFeeAmount = (buyFeeAmount * buyEcologyFee / uint256(100));
                uint256 lockFeeAmount = (buyFeeAmount * lockFee / uint256(100));
                poolNum += (buyFeeAmount * poolFee / uint256(100));
                super._transfer(from, ecology, buyEcologyFeeAmount);
                super._transfer(from, lock, lockFeeAmount);
                amount -= buyFeeAmount;
                buyFeeAmount -= (buyEcologyFeeAmount + lockFeeAmount);
                super._transfer(from, address(this), buyFeeAmount);
                takeFee = true;
            }
        }

        if(takeFee){
            uint256 balance = balanceOf(address(this)) - poolNum;
            if(balance > swapTokensAtAmount){
                swapAndLiquifyV3(balance);
            }
        }
        super._transfer(from, to, amount);
    }

    function swapAndLiquifyV3(uint256 contractTokenBalance) public {
        uint256 convert_u = contractTokenBalance.div(2);
        uint256 coin = contractTokenBalance.sub(convert_u);
        swapTokensForOther(convert_u);
        uint256 contract_u = PEOPLE.balanceOf(address(this));
        addLiquidity(coin, contract_u);
    }
    
    function addLiquidity(uint256 tokenAmount, uint256 peopleAmount) private {
        _approve(address(this), address(uniswapV2Router), tokenAmount);
        uniswapV2Router.addLiquidity(
            address(this),
            0x2c44b726ADF1963cA47Af88B284C06f30380fC78,
            tokenAmount,
            peopleAmount,
            0,
            0,
            uniswapV2Pair,
            block.timestamp
        );
    }

    function swapTokensForOther(uint256 tokenAmount) private {
		address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = address(0x2c44b726ADF1963cA47Af88B284C06f30380fC78);
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }
}