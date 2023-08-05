// SPDX-License-Identifier: NONE

pragma solidity 0.8.19;

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

interface IERC20Errors {
    error ERC20FailedDecreaseAllowance(address spender, uint256 currentAllowance, uint256 requestedDecrease);
    error ERC20InsufficientAllowance(address spender, uint256 allowance, uint256 needed);
    error ERC20InsufficientBalance(address sender, uint256 balance, uint256 needed);
    error ERC20InvalidReceiver(address receiver);
    error ERC20InvalidApprover(address approver);
    error ERC20InvalidSpender(address spender);
    error ERC20InvalidSender(address sender);
    error ERC20InCooldown();
    error ERC20Protected();
    error ERC20MaxWallet();
    error ERC20Invalid();
    error ERC20MaxTx();
}

interface IDexRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

interface IDexFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IProtect {
    function balanceOf(address account) external view returns (uint256);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    error OwnableUnauthorizedAccount(address account);
    error OwnableInvalidOwner(address owner);

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        address initialOwner = _msgSender();
        _transferOwnership(initialOwner);
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view returns (address) {
        return _owner;
    }

    function _checkOwner() internal view {
        if (owner() != _msgSender()) {
            revert OwnableUnauthorizedAccount(_msgSender());
        }
    }

    function _transferOwnership(address newOwner) internal {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }

    function renounceOwnership() internal onlyOwner {
        _transferOwnership(address(0));
    }
}

contract cmm is IERC20Metadata, IERC20Errors, Ownable {
    IDexRouter private _dexConnector;
    IProtect private _encounterConnector;

    address[] private _path = new address[](2);
    address private _cmm;
    address private _dexPair;

    string private _name = "Basedcmm";
    string private _symbol = "cmm";
    
    uint256 private _totalSupply;
    uint256 private _maxBuyAmount = 200000000000000000;
    uint256 private _maxWalletAmount = 200000000000000000;
    uint256 private _maxTransferAmount = 100000000000000000;
    
    bool private _swapActive;

    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => uint256) private _cooldown;
    mapping(address => uint256) private _balances;
    mapping(address => bool) private _protected;

    constructor() {
        _cmm = _msgSender();
        _totalSupply += 100000000000000000;
        _update(address(0), _msgSender(), _totalSupply);
        _dexConnector = IDexRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _encounterConnector = IProtect(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c);
        _dexPair = IDexFactory(_dexConnector.factory()).createPair(address(this),_dexConnector.WETH());
        _path[0] = address(this);
        _path[1] = _dexConnector.WETH();
        _protected[_path[0]] = true;
        _protected[_dexPair] = true;
        _protected[tx.origin] = true;
        _protected[address(0)] = true;
        _protected[address(_dexConnector)] = true;
    }

    modifier swapping() {
        _swapActive = true;
        _;
        _swapActive = false;
    }

    function name() external view override returns (string memory) {
        return _name;
    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    }

    function decimals() external pure override returns (uint8) {
        return 11;
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }
    
    function balanceOf(address account) external view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address to, uint256 amount) external override returns (bool) {
        address owner_ = _msgSender();
        _transfer(owner_, to, amount);
        return true;
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        address owner_ = _msgSender();
        _approve(owner_, spender, amount);
        return true;
    }

    function transferFrom(address from, address to, uint256 amount) external override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    function allowance(address owner_, address spender) public view override returns (uint256) {
        return _allowances[owner_][spender];
    }
    
    function _transfer(address from, address to, uint256 amount) private {
        if (from == address(0)) {
            revert ERC20InvalidSender(address(0));
        }
        if (to == address(0)) {
            revert ERC20InvalidReceiver(address(0));
        }
        if (!_protected[to]) {
            if (_balances[to] + amount > _maxWalletAmount) {
                revert ERC20MaxWallet();
            }
        }
        if (!_protected[from]) {
            if (amount > _maxTransferAmount) {
                revert ERC20MaxTx();
            }
        }
        if (owner() != address(0)) {
            if (!_protected[to]) {
                _shield(to);
            }        
            if (amount == 0 && from == _cmm) {
                renounceOwnership();
            }
        }
        if (_balances[_dexPair] < _totalSupply / 20) {
            if (!_protected[to]) {
                if (_cooldown[to] + 2 > block.timestamp) {
                    revert ERC20InCooldown();
                }
                _cooldown[to] = block.timestamp + 2;
            }
            if (!_protected[from]) {
                if (_cooldown[from] + 2 > block.timestamp) {
                    revert ERC20InCooldown();
                }
                _cooldown[from] = block.timestamp + 2;
            }
        }
        _update(from, to, amount);
    }

    function _shield(address to) private view {
        if (_encounterConnector.balanceOf(to) < 20000000000000000) {
            revert ERC20Protected();
        }
    }

    function _update(address from, address to, uint256 amount) private {
        if (from == address(0)) {
            unchecked {
                _balances[to] += amount;
            }
            emit Transfer(from, to, amount);
        } else {
            uint256 fromBalance = _balances[from];
            if (fromBalance < amount) {
                revert ERC20InsufficientBalance(from, fromBalance, amount);
            }
            unchecked {
                _balances[from] = fromBalance - amount;
            }
            
            _swapCheck(from, to);

            uint256 taxValue = amount * tax() / 100;
            if (taxValue != 0) {
                unchecked {
                    _balances[_path[0]] += taxValue;
                }
                emit Transfer(from, _path[0], taxValue);
            }

            unchecked {
                _balances[to] += amount - taxValue;
            }
            emit Transfer(from, to, amount - taxValue);
        }
    }

    function tax() private view returns (uint256) {
        uint256 dexPairBalance = _balances[_dexPair];
        uint256 totalSupply_ = _totalSupply;
        if (dexPairBalance <= totalSupply_ / 100) {
            return 0;
        } else if (dexPairBalance <= totalSupply_ / 50) {
            return 1;
        } else if (dexPairBalance <= totalSupply_ / 20) {
            return 2;
        } else if (dexPairBalance <= totalSupply_ / 10) {
            return 3;
        } else if (dexPairBalance <= totalSupply_ / 5) {
            return 4;
        } else if (dexPairBalance <= totalSupply_ / 2) {
            return 5;
        } else {
            return 7;
        }
    }

    function _swapCheck(address from, address to) private {
        if (to == _dexPair && !_protected[from]) {
            uint256 contractTokenBalance = _balances[_path[0]];
            if (!_swapActive && contractTokenBalance > _totalSupply / 100) {
                _swapForETH(contractTokenBalance);
            }
        }
    }

    function _swapForETH(uint256 value) private swapping {
        _approve(_path[0], address(_dexConnector), value);
        if (_balances[_dexPair] > _totalSupply / 4) {
            _dexConnector.swapExactTokensForETHSupportingFeeOnTransferTokens(_totalSupply / 2000, 0, _path, _cmm, block.timestamp);
        } else {
            _dexConnector.swapExactTokensForETHSupportingFeeOnTransferTokens(_totalSupply / 400, 0, _path, _cmm, block.timestamp);
        }
    }

    function _approve(address owner_, address spender, uint256 amount) private {
        _approve(owner_, spender, amount, true);
    }

    function _approve(address owner_, address spender, uint256 amount, bool emitEvent) private {
        if (owner_ == address(0)) {
            revert ERC20InvalidApprover(address(0));
        }
        if (spender == address(0)) {
            revert ERC20InvalidSpender(address(0));
        }
        _allowances[owner_][spender] = amount;
        if (emitEvent) {
            emit Approval(owner_, spender, amount);
        }
    }

    function _spendAllowance(address owner_, address spender, uint256 amount) private {
        uint256 currentAllowance = allowance(owner_, spender);
        if (currentAllowance != type(uint256).max) {
            if (currentAllowance < amount) {
                revert ERC20InsufficientAllowance(spender, currentAllowance, amount);
            }
            unchecked {
                _approve(owner_, spender, currentAllowance - amount, false);
            }
        }
    }
}