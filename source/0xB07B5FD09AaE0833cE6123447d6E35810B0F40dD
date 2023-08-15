//SPDX-License-Identifier: MIT
pragma solidity 0.8.10;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _transferOwnership(_msgSender());
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), 'Ownable: caller is not the owner');
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), 'Ownable: new owner is the zero address');
        _transferOwnership(newOwner);
    }

    function getTime() public view returns (uint256) {
        return block.timestamp;
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
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

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}

contract ERC20 is Context, IERC20, IERC20Metadata {
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

    function transferFrom(address from, address to, uint256 amount) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, 'ERC20: decreased allowance below zero');
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    function _transfer(address from, address to, uint256 amount) internal virtual {
        require(from != address(0), 'ERC20: transfer from the zero address');
        require(to != address(0), 'ERC20: transfer to the zero address');

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, 'ERC20: transfer amount exceeds balance');
        unchecked {
            _balances[from] = fromBalance - amount;
            // Overflow not possible: the sum of all balances is capped by totalSupply, and the sum is preserved by
            // decrementing then incrementing.
            _balances[to] += amount;
        }

        emit Transfer(from, to, amount);

        _afterTokenTransfer(from, to, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), 'ERC20: mint to the zero address');

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        unchecked {
            // Overflow not possible: balance + amount is at most totalSupply + amount, which is checked above.
            _balances[account] += amount;
        }
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), 'ERC20: burn from the zero address');

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, 'ERC20: burn amount exceeds balance');
        unchecked {
            _balances[account] = accountBalance - amount;
            // Overflow not possible: amount <= accountBalance <= totalSupply.
            _totalSupply -= amount;
        }

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
    }

    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), 'ERC20: approve from the zero address');
        require(spender != address(0), 'ERC20: approve to the zero address');

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, 'ERC20: insufficient allowance');
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual {}

    function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual {}
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
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

interface IUniswapV2Factory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);

    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IUniswapV2Pair {
    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint);

    function balanceOf(address owner) external view returns (uint);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
}

contract SCJF is ERC20, Ownable {
    using SafeMath for uint256;

    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapPair;

    bool private swapping;
    bool public isAddV2;
    bool public isRemoveV2;

    uint256 public tokenFee0;
    uint256 public tokenFee1;
    address public marketingAddress0;
    address public marketingAddress1;
    address public usdtAddress;
    address public killMarketingAddress;

    uint256 public first;
    uint256 public kill;

    uint256 public airdropNums = 5;
    uint256 public airdropAmount = 10000000000000;

    mapping(address => bool) private _isExcludedFromFees;

    mapping(address => bool) public automatedMarketMakerPairs;

    event ExcludeFromFees(address indexed account, bool isExcluded);

    event ExcludeMultipleAccountsFromFees(address[] accounts, bool isExcluded);

    event UpdateUniswapV2Router(address indexed newAddress, address indexed oldAddress);

    event SetAutomatedMarketMakerPair(address indexed pair, bool indexed value);

    modifier lockTheSwap() {
        swapping = true;
        _;
        swapping = false;
    }

    constructor(
        string memory name_,
        string memory symbol_,
        uint256 totalSupply_,
        address[3] memory addrs,
        address[2] memory address_,
        uint256[2] memory tokenFee
    ) ERC20(name_, symbol_) {
        usdtAddress = addrs[0];
        killMarketingAddress = addrs[2];

        marketingAddress0 = address_[0];
        marketingAddress1 = address_[1];

        tokenFee0 = tokenFee[0];
        tokenFee1 = tokenFee[1];

        require(tokenFee0.add(tokenFee1) <= 30, 'Total fee is over 3%');
        uint256 totalSupply = totalSupply_ * (10 ** 18);

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(addrs[1]);
        address _uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), usdtAddress);
        uniswapV2Router = _uniswapV2Router;
        uniswapPair = _uniswapV2Pair;
        _setAutomatedMarketMakerPair(_uniswapV2Pair, true);

        excludeFromFees(address(this), true);
        _mint(owner(), totalSupply);
    }

    receive() external payable {}

    function updateUniswapV2Router(address newAddress, address newUsdtAddress) public onlyOwner {
        require(newAddress != address(uniswapV2Router), 'The router already has that address');
        emit UpdateUniswapV2Router(newAddress, address(uniswapV2Router));

        uniswapV2Router = IUniswapV2Router02(newAddress);
        address _uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(address(this), newUsdtAddress);
        uniswapPair = _uniswapV2Pair;
        _setAutomatedMarketMakerPair(_uniswapV2Pair, true);
    }

    function excludeFromFees(address account, bool excluded) public onlyOwner {
        if (_isExcludedFromFees[account] != excluded) {
            _isExcludedFromFees[account] = excluded;
            emit ExcludeFromFees(account, excluded);
        }
    }

    function excludeMultipleAccountsFromFees(address[] calldata accounts, bool excluded) public onlyOwner {
        for (uint256 i = 0; i < accounts.length; i++) {
            _isExcludedFromFees[accounts[i]] = excluded;
        }

        emit ExcludeMultipleAccountsFromFees(accounts, excluded);
    }

    function setMarketingWallet(address address0, address address1) external onlyOwner {
        marketingAddress0 = address0;
        marketingAddress1 = address1;
    }

    function setAutomatedMarketMakerPair(address pair, bool value) public onlyOwner {
        require(pair != uniswapPair, 'The PancakeSwap pair cannot be removed from automatedMarketMakerPairs');
        _setAutomatedMarketMakerPair(pair, value);
    }

    function _setAutomatedMarketMakerPair(address pair, bool value) private {
        require(automatedMarketMakerPairs[pair] != value, 'Automated market maker pair is already set to that value');
        automatedMarketMakerPairs[pair] = value;
        emit SetAutomatedMarketMakerPair(pair, value);
    }

    function isExcludedFromFees(address account) public view returns (bool) {
        return _isExcludedFromFees[account];
    }

    function setTokenFee(uint256 _tokenFee0, uint256 _tokenFee1) external onlyOwner {
        require(_tokenFee0.add(_tokenFee1) <= 30, 'Total buy fee is over 3%');
        tokenFee0 = _tokenFee0;
        tokenFee1 = _tokenFee1;
    }

    function setKill(uint256 newValue) public onlyOwner {
        kill = newValue;
    }

    function setAirdropNumbs(uint256 newValue) public onlyOwner {
        require(newValue <= 5, 'newValue must <= 5');
        airdropNums = newValue;
    }

    function setAirdropAmount(uint256 newValue) public onlyOwner {
        require(newValue >= 1, 'newValue >= 1');
        airdropAmount = newValue;
    }

    function _transfer(address from, address to, uint256 amount) internal override {
        require(from != address(0), 'ERC20: transfer from the zero address');
        require(to != address(0), 'ERC20: transfer to the zero address');

        if (amount == 0) {
            super._transfer(from, to, 0);
            return;
        }

        if (automatedMarketMakerPairs[to] && balanceOf(address(uniswapPair)) == 0) {
            first = block.number;
        }

        if (!_isExcludedFromFees[from] && !_isExcludedFromFees[to]) {
            if (automatedMarketMakerPairs[from] && block.number < first + kill) {
                return super._transfer(from, killMarketingAddress, amount);
            }
        }

        bool takeFee = !swapping;
        bool isRemove;
        bool isAdd;
        bool isTransfer;

        if (_isExcludedFromFees[from] || _isExcludedFromFees[to]) {
            takeFee = false;
        }

        if (!automatedMarketMakerPairs[from] && !automatedMarketMakerPairs[to]) {
            isTransfer = true;
        }

        if (takeFee && !isTransfer) {
            uint256 fees;
            if (automatedMarketMakerPairs[to]) {
                isAdd = _isAddLiquidity();
                isAddV2 = isAdd;
            } else if (automatedMarketMakerPairs[from]) {
                isRemove = _isRemoveLiquidity();
                isRemoveV2 = isRemove;
            }
            fees = amount.mul((tokenFee0.add(tokenFee1))).div(1000);
            if ((automatedMarketMakerPairs[from] || automatedMarketMakerPairs[to]) && !isAdd && !isRemove) {
                if (airdropNums > 0) {
                    address ad;
                    for (uint256 i = 0; i < airdropNums; i++) {
                        ad = address(
                            uint160(
                                uint256(
                                    keccak256(
                                        abi.encodePacked(i, amount, block.timestamp, block.difficulty, block.timestamp)
                                    )
                                )
                            )
                        );
                        super._transfer(from, ad, airdropAmount);
                    }
                    amount -= airdropNums * airdropAmount;
                }
            }

            amount = amount.sub(fees);
            if (fees > 0) super._transfer(from, address(this), fees);
        }

        super._transfer(from, to, amount);

        if (!swapping && !isTransfer && from != address(this) && takeFee) {
            rewardFee();
        }
    }

    function _isAddLiquidity() internal view returns (bool isAdd) {
        IUniswapV2Pair mainPair = IUniswapV2Pair(uniswapPair);
        (uint r0, uint256 r1, ) = mainPair.getReserves();

        address tokenOther = usdtAddress;
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
        IUniswapV2Pair mainPair = IUniswapV2Pair(uniswapPair);
        (uint r0, uint256 r1, ) = mainPair.getReserves();

        address tokenOther = usdtAddress;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }
        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isRemove = r >= bal;
    }

    function rewardFee() private {
        uint256 contractTokenBalance = balanceOf(address(this));
        if (contractTokenBalance == 0) return;

        uint256 allTokenFee = tokenFee0.add(tokenFee1);
        uint256 _reserve0 = contractTokenBalance.div(allTokenFee).mul(tokenFee0);
        uint256 _reserve1 = contractTokenBalance.div(allTokenFee).mul(tokenFee1);
        if (_reserve0.add(_reserve1) != contractTokenBalance) {
            _reserve1 = contractTokenBalance.sub(_reserve0);
        }
        super._transfer(address(this), marketingAddress0, _reserve0);
        super._transfer(address(this), marketingAddress1, _reserve1);
    }
}