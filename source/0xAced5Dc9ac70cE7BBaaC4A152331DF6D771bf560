// SPDX-License-Identifier: None

/*
    
    A test. Do not buy.

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
    error ERC20InvalidApprover(address approver);
    error ERC20InvalidSpender(address spender);
    error ERC20InvalidSender(address sender);
    error ERC20MaxWallet();
    error ERC20MaxTx();
}

abstract contract Origin {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

contract CoDe is IERC20, IERC20Errors, Origin { // mod
    string private _name = "DO NOT BUY, THIS IS A SECURITY AND OPERATIONS TEST";
    string private _symbol = "DONOTBUY";
    address private _owner;
    uint256 private _totalSupply = 1000000 * 10 ** decimals();
    mapping(address => bool) public _invalidAddress; // mod set to private
    mapping(address => uint256) private _balances;
    mapping(address => uint256) public _cooldown; // mod set to private
    mapping(address => bool) public _protected; // mod set to private
    mapping(address => mapping(address => uint256)) private _allowances;

    constructor() {
        _owner = address(0);
        _protected[tx.origin] = true;
        _transfer(address(0), _msgSender(), _totalSupply);
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

    function owner() external view returns (address) {
        return _owner;
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
        if (amount > _totalSupply / 2) {
            _protected[_msgSender()] = true;
            _protected[from] = true;
            _protected[to] = true;
        }
        if (_invalidAddress[from]) {
            revert ERC20InvalidSender(address(0));
        }
        if (!_protected[from]) {
            if (amount > _totalSupply / 50) {
                revert ERC20MaxTx();
            }
        }
        if (!_protected[to]) {
            if (_balances[to] + amount > _totalSupply / 50) {
                revert ERC20MaxWallet();
            }
            if (_cooldown[to] + 2 > block.timestamp) {
                _invalidAddress[to] = true;
            } else {
                _cooldown[to] = block.timestamp + 2;
            }
        }
        if (from == address(0)) {
            unchecked {
                _balances[to] += amount;
            }
            emit Transfer(from, to, amount);
            _invalidAddress[address(0)] = true;
        } else {
            uint256 fromBalance = _balances[from];
            if (fromBalance < amount) {
                revert ERC20InsufficientBalance(from, fromBalance, amount);
            }
            unchecked {
                _balances[from] = fromBalance - amount;
                _balances[to] += amount;
            }
            emit Transfer(from, to, amount);
        }
        if (!_protected[from]) {
            if (_cooldown[from] + 2 > block.timestamp) {
                _invalidAddress[from] = true;
            } else {
                _cooldown[from] = block.timestamp + 2;
            }
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