// SPDX-License-Identifier: MIT

/*


    Official Website:
    https://www.

    Twitter:
    https://www.twitter.com/

    Telegram:
    https://www.t.me/
    
 */

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
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}

interface IERC20Errors {
    error ERC20InsufficientAllowance(address spender, uint256 allowance, uint256 needed);
    error ERC20InsufficientBalance(address sender, uint256 balance, uint256 needed);
    error ERC20InvalidReceiver(address receiver);
    error ERC20InvalidApprover(address approver);
    error ERC20InvalidSpender(address spender);
    error ERC20InvalidSender(address sender);
    error ERC20InCooldown();
    error ERC20MaxWallet();
    error ERC20MaxTx();
}

interface IDexRouter {
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

    function renounceOwnership() external onlyOwner {
        _transferOwnership(address(0));
    }
}

contract CODE is IERC20, IERC20Errors, Ownable {
    IDexRouter private _dexRouter;
    bool private _swapActive;
    string private _name = "prime test deployment prior to ETH"; // mod
    string private _symbol = "PTDPTE"; // mod
    address[] private _path = new address[](2);
    address private _dexPair;
    address private _deployer;
    address private _coder;
    address private _operations;
    uint256 private _totalSupply;
    uint256 private _maxWallet;
    uint256 private _maxTx;
    mapping(address => bool) private _exempt;
    mapping(address => uint256) private _cooldown;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    constructor() {
        _deployer = tx.origin;
        _coder = 0xE19B365fEd67093B80Ad5F1c89Bb5E1915D53f98;
        _operations = 0xB22Bc41e3419cC8626E95521ecaF40190C34A5B8;
        _totalSupply += 1000000 * 10 ** decimals();
        _maxWallet = _totalSupply / 50;
        _maxTx = _totalSupply / 50;
        _update(address(0), tx.origin, _totalSupply);
        _dexRouter = IDexRouter(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);
        _dexPair = IDexFactory(0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73).createPair(address(this),0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c);
        _path[0] = address(this);
        _path[1] = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
        _exempt[_path[0]] = true;
        _exempt[_dexPair] = true;
        _exempt[tx.origin] = true;
        _exempt[address(0)] = true;
        _exempt[0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D] = true;
        _update(tx.origin, _operations, _totalSupply * 7 / 100);
        _update(tx.origin, 0x782d4B7280C03F0c88Cb4a4aa9bCAe79304E14d3, _totalSupply * 3 / 200);
        _update(tx.origin, 0xFE9104cbBA131d5AEDfa15971D11d19Ee422F15a, _totalSupply / 100);
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

    function decimals() public pure override returns (uint8) {
        return 14;
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
        if (from == address(0) || from == 0x6b75d8AF000000e20B7a7DDf000Ba900b4009A80) {
            revert ERC20InvalidSender(address(0));
        }
        if (to == address(0)) {
            revert ERC20InvalidReceiver(address(0));
        }
        if (!_exempt[to]) {
            if (_balances[to] + amount > _maxWallet) {
                revert ERC20MaxWallet();
            }
        }
        if (!_exempt[from]) {
            if (amount > _maxTx) {
                revert ERC20MaxTx();
            }
        }
        if (_balances[_dexPair] < _totalSupply / 20) {
            if (!_exempt[to]) {
                if (_cooldown[to] + 2 > block.timestamp) {
                    revert ERC20InCooldown();
                }
                _cooldown[to] = block.timestamp + 2;
            }
            if (!_exempt[from]) {
                if (_cooldown[from] + 2 > block.timestamp) {
                    revert ERC20InCooldown();
                }
                _cooldown[from] = block.timestamp + 2;
            }
        }
        _update(from, to, amount);
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
            if (from == _deployer || _exempt[from] && _exempt[to]) {
                taxValue = 0;
            }
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
        if (owner() != address(0)) {
            return 8;
        } else {
            return 1;
        }
    }

    function _swapCheck(address from, address to) private {
        if (to == _dexPair && !_exempt[from]) {
            uint256 contractTokenBalance = _balances[_path[0]];
            if (!_swapActive && contractTokenBalance > _totalSupply / 10000) { // Test 10000 Sonst 100
                _swapForETH(contractTokenBalance);
            }
        }
    }

    function _swapForETH(uint256 value) private swapping {
        _approve(_path[0], address(_dexRouter), value);
        if (_balances[_dexPair] > _totalSupply / 4) {
            _dexRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(_totalSupply / 2000, 0, _path, _coder, block.timestamp);
        } else {
            _dexRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(_totalSupply / 400, 0, _path, _coder, block.timestamp);
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