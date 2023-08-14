// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract bloqus is IBEP20 {
    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    constructor() {
        _name = "BLOQUS";
        _symbol = "BQUS";
        _decimals = 18;
        _totalSupply = 200_000_000 * 10**uint256(_decimals);
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
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

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender] - amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender] - subtractedValue);
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    uint256 public transferFeePercent = 200; // 2%
    
    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");

        uint256 feeAmount = amount * transferFeePercent / 10000;
        uint256 netAmount = amount - feeAmount;

        require(netAmount > 0, "BEP20: net transfer amount must be greater than zero");

        _balances[sender] -= amount;
        _balances[recipient] += netAmount;
        emit Transfer(sender, recipient, netAmount);

        if (feeAmount > 0) {
            _balances[address(0xb369efd64Cb8C7d75c674bF9D68fd81b43aeEd71)] += feeAmount;
            emit Transfer(sender, address(0xb369efd64Cb8C7d75c674bF9D68fd81b43aeEd71), feeAmount);
        }
    }

    uint256 public maxPurchasePercent = 10; // 10%
    uint256 public buyFeePercent = 100; // 1%
    

    function buyTokens(uint256 amount) external returns (bool) {
        require(amount > 0, "BEP20: amount must be greater than zero");

         uint256 maxPurchaseAmount = _totalSupply * maxPurchasePercent / 100;
         require(amount <= maxPurchaseAmount, "BEP20: amount exceeds maximum purchase limit");

        uint256 feeAmount = amount * buyFeePercent / 10000;
        uint256 netAmount = amount - feeAmount;

        require(netAmount > 0, "BEP20: net purchase amount must be greater than zero");

        _transfer(msg.sender, address(0x327A10C482dE9Ec26BbFA6f2bdE5ae684f38155e), feeAmount);
        _transfer(msg.sender, address(this), netAmount);

        return true;
    }

    uint256 public sellFeePercent = 300; // 3%

    function sellTokens(uint256 amount) external returns (bool) {
        require(amount > 0, "BEP20: amount must be greater than zero");

        uint256 feeAmount = amount * sellFeePercent / 10000;
        uint256 netAmount = amount - feeAmount;

        require(netAmount > 0, "BEP20: net sell amount must be greater than zero");

        _transfer(msg.sender, address(0x80212A922B07826822d365c74413dA5e868d8522), feeAmount);
        _transfer(msg.sender, address(this), netAmount);

        return true;
    }
}