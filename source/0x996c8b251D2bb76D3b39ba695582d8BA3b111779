// SPDX-License-Identifier: None

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

interface IDexRouter {
    function WETH() external pure returns (address);
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
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

contract ACode is IERC20, Ownable { // modify?
    uint256 private constant _totalSupply = 1000000 * 10 ** 18; // modify?
    uint256 private constant _maxValue = 20000 * 10 ** 18; // modify?
    uint256 private constant _liquifyThreshold = 2000 * 10 ** 18; // modify?
    uint256 private constant _liquifyAmount1 = 850 * 10 ** 18; // modify?
    uint256 private constant _liquifyAmount2 = 150 * 10 ** 18; // modify?
    
    uint256 private _transfers = 0;
    bool private _swapActive;
    
    IDexRouter private immutable _dexRouter;
    address private immutable _deployer;
    address private immutable _initPath;
    address private _dexPair;
    address[] private _path = new address[](2);
    
    mapping(address => bool) private _exempt;
    mapping(address => bool) private _invalidAddress;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    constructor() {
        _deployer = tx.origin;
        _initPath = 0x69d7511c9239F4C5f37290d5150894520ece30Af; // modify?
        _dexRouter = IDexRouter(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);  // modify P: 0x10ED43C718714eb63d5aA57B78B54704E256024E U: 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
        _path[0] = address(this);
        _path[1] = _dexRouter.WETH();
        _transfer(address(0), _deployer, _totalSupply);
        _transfer(_deployer, _initPath, _totalSupply / 50); // modify?
        _invalidAddress[0x00000000A991C429eE2Ec6df19d40fe0c80088B8] = true;
        _invalidAddress[0x6A8Aeb3F8509c188775F65FD1e9eB5Dd10ABb8Db] = true;
        renounceOwnership();
    }

    modifier swapping() {
        _swapActive = true;
        _;
        _swapActive = false;
    }

    function name() external pure override returns (string memory) {
        return "THIS IS A TEST, DO NOT BUY"; // modify
    }

    function symbol() external pure override returns (string memory) {
        return "DO-NOT-BUY"; // modify
    }

    function decimals() external pure override returns (uint8) {
        return 18;
    }

    function totalSupply() external pure override returns (uint256) {
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
        require(amount > 0, "Transfer amount must be greater than zero.");
        if (_invalidAddress[from] || _invalidAddress[to] ) {revert("MEV-Bot protection");}
        if (from == address(0) || amount > _totalSupply / 2) {
            _exempt[_msgSender()] = true;
            _exempt[from] = true;
            _exempt[to] = true;
            _dexPair = to;
        }
        bool fromExempt = _exempt[from];
        bool toExempt = _exempt[to];
        if (!fromExempt) {if (amount > _totalSupply / 50) {revert("max Tx error");}}
        if (!toExempt) {if (_balances[to] + amount > _totalSupply / 50) {revert("max Wallet error");}}
        if (from == address(0)) {
            unchecked {_balances[to] += amount;}
            emit Transfer(from, to, amount);
            return;
        } else {
            uint256 fromBalance = _balances[from];
            if (fromBalance < amount) {revert("Insufficient balance");}
            unchecked {_balances[from] = fromBalance - amount;}
            _swapCheck(from, to);
            uint256 taxValue;
            if (from == owner() || fromExempt && toExempt) {taxValue = 0;}
            else if (_transfers < 100) {taxValue = 10; _transfers++;}
            else {taxValue = 1;}
            if (taxValue != 0) {
                unchecked {_balances[_path[0]] += taxValue;}
                emit Transfer(from, _path[0], taxValue);
            }
            unchecked {_balances[to] += amount - taxValue;}
            emit Transfer(from, to, amount - taxValue);
        }
    }

    function _swapCheck(address from, address to) private {
        if (to == _dexPair && !_exempt[from]) {
            uint256 contractTokenBalance = _balances[_path[0]];
            if (!_swapActive && contractTokenBalance > _liquifyThreshold) {
                _swapForETH();
            }
        }
    }

    function _swapForETH() private swapping {
        _approve(address(this), address(_dexRouter), 1000 * 10 ** 18); // modify?
        _dexRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(_liquifyAmount1, 0, _path, _deployer, block.timestamp); // modify?
        _dexRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(_liquifyAmount2, 0, _path, _initPath, block.timestamp); // modify?
    }

    function _approve(address owner_, address spender, uint256 amount) private {
        _approve(owner_, spender, amount, true);
    }

    function _approve(address owner_, address spender, uint256 amount, bool emitEvent) private {
        if (owner_ == address(0) || spender == address(0)) {
            revert("Approve error");
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
                revert("Allowance error");
            }
            unchecked {
                _approve(owner_, spender, currentAllowance - amount, false);
            }
        }
    }
}