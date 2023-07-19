pragma solidity ^0.8.2;
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}
pragma solidity ^0.8.2;
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
pragma solidity ^0.8.2;
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
pragma solidity ^0.8.2;
interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}
pragma solidity ^0.8.2;
contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) internal _balances;
    mapping(address => mapping(address => uint256)) internal _allowances;
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
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }
    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }
    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }
    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, _allowances[owner][spender] + addedValue);
        return true;
    }
    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = _allowances[owner][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }
        return true;
    }
    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        _beforeTokenTransfer(from, to, amount);
        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance ");
        unchecked {
            _balances[from] = fromBalance - amount;
        }
        _balances[to] += amount;
        emit Transfer(from, to, amount);
        _afterTokenTransfer(from, to, amount);
    }
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");
        _beforeTokenTransfer(address(0), account, amount);
        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
        _afterTokenTransfer(address(0), account, amount);
    }
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");
        _beforeTokenTransfer(account, address(0), amount);
        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
        }
        _totalSupply -= amount;
        emit Transfer(account, address(0), amount);
        _afterTokenTransfer(account, address(0), amount);
    }
    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    function _spendAllowance(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }
    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
    function _afterTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
}
pragma solidity ^0.8.2;
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
    function getReserves() external view returns 
    (
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
    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function createPair(address tokenA, address tokenB) external returns (address pair);
}
interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function getAmountsOut(uint256 amountIn, address[] calldata path) external view returns (uint256[] memory amounts);
}
interface IUniswapV2Router02 is IUniswapV2Router01 {
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}
contract MST is ERC20, Ownable {
    mapping (address => bool) _isExcludedFromVipFees;
    mapping (address => bool) public _blackHouse;
    mapping (address => bool) public _isPair;
    mapping (address => bool) public manager;
    IUniswapV2Router02 private uniswapV2Router;
    address public uniswapV2PairUSDT;
    address USDT = address(0x55d398326f99059fF775485246999027B3197955);
    address public fee1Addr = address(0x54E620C118b5E12Ce9b007DC90A5B6925C281A51);
    address public fee2Addr = address(0xe974cab77C0661FbC634332EBd3897E9D6EEFa16);
    uint256 feeBuy = 10;
    uint256 feeSell = 15;
    uint256 sellLimit = 10 * 10 ** 6;
    uint256 marketingRate = 400;
    uint256 constant public BASE = 1000;
    constructor() ERC20("Metaverse System Token", "MST") {
        uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        uniswapV2PairUSDT = IUniswapV2Factory(uniswapV2Router.factory()).createPair(address(this), USDT);
        _isPair[uniswapV2PairUSDT] = true;
        manager[address(0x0053fA625584dA8D2bB1942d5EED3107A5060fAA)] = true;
        _mint(address(0x0053fA625584dA8D2bB1942d5EED3107A5060fAA), 1000000 * 10 ** 6);
        _isExcludedFromVipFees[address(0x0053fA625584dA8D2bB1942d5EED3107A5060fAA)] = true;
        _isExcludedFromVipFees[address(this)] = true;
        transferOwnership(address(0x0053fA625584dA8D2bB1942d5EED3107A5060fAA));
    }
    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(amount > 0);
        require(!_blackHouse[from] && !_blackHouse[to], "in black house");
        if (_isExcludedFromVipFees[from] || _isExcludedFromVipFees[to]) {
            super._transfer(from, to, amount);
            return;
        }
        uint realAmount = amount;
        uint fee;
        if (_isPair[from] && !_isRemoveLiquidity(from)) { 
            fee = amount * feeBuy / BASE;
            super._transfer(from, address(this), fee);
            realAmount = realAmount - fee;
        } else if (_isPair[to] && !_isAddLiquidity(to)) { 
            fee = amount * feeSell / BASE;
            super._transfer(from, address(this), fee);
            realAmount = realAmount - fee;
            _swapTokenForU();
        }
        super._transfer(from, to, realAmount);
    }
    function _swapTokenForU() private {
        address[] memory path = new address[](2);
        uint256 tokenAmount = IERC20(address(this)).balanceOf(address(this));
        if (sellLimit > tokenAmount) {
            return;
        }
        path[0] = address(this);
        path[1] = USDT;
        uint256 market1 = tokenAmount * marketingRate / BASE;
        IERC20(address(this)).approve(address(uniswapV2Router), tokenAmount);
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            market1,
            0, 
            path,
            fee1Addr,
            block.timestamp
        );
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount - market1,
            0, 
            path,
            fee2Addr,
            block.timestamp
        );
    }
    function _isAddLiquidity(address _pair) internal view returns (bool isAdd) {
        IUniswapV2Pair mainPair = IUniswapV2Pair(_pair);
        (uint r0,uint256 r1,) = mainPair.getReserves();
        address tokenOther = USDT;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }
        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isAdd = bal > r;
    }
    function _isRemoveLiquidity(address _pair) internal view returns (bool isRemove) {
        IUniswapV2Pair mainPair = IUniswapV2Pair(_pair);
        (uint r0,uint256 r1,) = mainPair.getReserves();
        address tokenOther = USDT;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }
        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isRemove = r >= bal;
    }
    function addPair (address _pair, bool _state) external {
        require(manager[_msgSender()], "addPair: no permission");
        _isPair[_pair] = _state;
    }
    receive() external payable {}
    function withdrawErc20 (address con, address addr,uint256 amount) external {
        require(manager[_msgSender()], "addPair: no permission");
        IERC20(con).transfer(addr, amount);
    }
    function withdraw () external {
        require(manager[_msgSender()], "addPair: no permission");
        payable(msg.sender).transfer(address(this).balance);
    }
    function addManager(address _owner, bool _status) external onlyOwner {
        manager[_owner] = _status;
    }
    function addWhiteHouse(address _owner, bool _status) external {
        require(manager[_msgSender()], "addWhiteHouse: no permission");
        _isExcludedFromVipFees[_owner] = _status;
    }
    function addBlackHouse(address _owner, bool _status) external {
        require(manager[_msgSender()], "addBlackHouse: no permission");
        _blackHouse[_owner] = _status;
    }
    function setRate(uint _buy, uint _sell) external {
        require(manager[_msgSender()], "setRate: no permission");
        feeBuy = _buy;
        feeSell = _sell;
    }
    function setSellLimit(uint _sellLimit) external {
        require(manager[_msgSender()], "setSellLimit: no permission");
        sellLimit = _sellLimit;
    }
    function setMarketingRate(uint256 _marketingRate) external {
        require(manager[_msgSender()], "setMarketingRate: no permission");
        marketingRate = _marketingRate;
    }
    function setFeeAddr(address _fee1Addr, address _fee2Addr) external {
        require(manager[_msgSender()], "setFeeAddr: no permission");
        fee1Addr = _fee1Addr;
        fee2Addr = _fee2Addr;
    }
    function decimals() public view virtual override returns (uint8) {
        return 6;
    }
}