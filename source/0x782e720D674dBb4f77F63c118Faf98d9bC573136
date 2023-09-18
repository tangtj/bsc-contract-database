// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

contract AIMB is IERC20 {
    string public constant name = "AIMB";
    string public constant symbol = "AIMB";
    uint8 public constant decimals = 18;
    uint256 private _totalSupply;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) public ExcludedFromFees;
    address public owner;
    uint256 public buyFee = 10; // 5% fee
    uint256 public sellFee = 10; // 3% fee

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    constructor(uint256 initialSupply) {
        owner = msg.sender;
        _mint(owner, initialSupply);
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account)
        external
        view
        override
        returns (uint256)
    {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount)
        external
        override
        returns (bool)
    {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address _owner, address spender)
        external
        view
        override
        returns (uint256)
    {
        return _allowances[_owner][spender];
    }

    function approve(address spender, uint256 amount)
        external
        override
        returns (bool)
    {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external override returns (bool) {
        uint256 currentAllowance = _allowances[sender][msg.sender];
        require(
            currentAllowance >= amount,
            "Transfer amount exceeds allowance"
        );
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, currentAllowance - amount);
        return true;
    }

    function mint(uint256 amount) external onlyOwner {
        _mint(msg.sender, amount);
    }

    function burn(uint256 amount) external {
        _burn(msg.sender, amount);
    }

    function setBuyFee(uint256 fee) external onlyOwner {
        buyFee = fee;
    }

    function setSellFee(uint256 fee) external onlyOwner {
        sellFee = fee;
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal {
        require(sender != address(0), "Sender cannot be the zero address");
        require(
            recipient != address(0),
            "Recipient cannot be the zero address"
        );
        require(_balances[sender] >= amount, "Not enough balance");

        uint256 feeAmount;
        if (ExcludedFromFees[sender] || ExcludedFromFees[recipient]) {
            feeAmount = 0;
        } else {
            if (recipient == owner) {
                feeAmount = (amount * sellFee) / 100;
            } else if (sender == owner) {
                feeAmount = (amount * buyFee) / 100;
            } else {
                feeAmount = 0;
            }
        }

        uint256 sendAmount = amount - feeAmount;
        _balances[sender] -= amount;
        _balances[recipient] += sendAmount;
        _balances[owner] += feeAmount;

        emit Transfer(sender, recipient, sendAmount);
        if (feeAmount > 0) {
            emit Transfer(sender, owner, feeAmount);
        }
    }

    function _approve(
        address _owner,
        address spender,
        uint256 amount
    ) internal {
        require(_owner != address(0), "Owner cannot be the zero address");
        require(spender != address(0), "Spender cannot be the zero address");

        _allowances[_owner][spender] = amount;
        emit Approval(_owner, spender, amount);
    }

    function _mint(address account, uint256 amount) internal {
        require(account != address(0), "Cannot mint to the zero address");

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "Cannot burn from the zero address");
        require(_balances[account] >= amount, "Not enough tokens to burn");
        _balances[account] -= amount;
        _totalSupply -= amount;
        emit Transfer(account, address(0), amount);
    }

    function SetExcludeFromFee(address exclude, bool value) public onlyOwner {
        ExcludedFromFees[exclude] = value;
    }
}