// SPDX-License-Identifier: MIT
//t.me/GogoGadgetBSC
//  _____                           _              _____           _            _   
// |_   _|                         | |            / ____|         | |          | |  
//   | |  _ __  ___ _ __   ___  ___| |_ ___  _ __| |  __  __ _  __| | __ _  ___| |_ 
//   | | | '_ \/ __| '_ \ / _ \/ __| __/ _ \| '__| | |_ |/ _` |/ _` |/ _` |/ _ \ __|
//  _| |_| | | \__ \ |_) |  __/ (__| || (_) | |  | |__| | (_| | (_| | (_| |  __/ |_ 
// |_____|_| |_|___/ .__/ \___|\___|\__\___/|_|   \_____|\__,_|\__,_|\__, |\___|\__|
//                | |                                                __/ |         
//                |_|                                               |___/          

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

contract InspectorGadget is IBEP20 {
    string public name = "Inspector Gadget";
    string public symbol = "GOGO";
    uint8 public decimals = 18;
    uint256 private _totalSupply;
    uint256 private _totalSupplyAvailable;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    address private _owner;
    address private constant _taxWallet = 0xdBe50Fee7DcD6F035BEb07148F82497388cB2903;
    uint256 private constant _taxPercentage = 4;
    mapping(address => uint256) private _lastTransactionTime;

    modifier onlyOwner() {
        require(msg.sender == _owner, "Only the owner can call this function");
        _;
    }

    modifier onlyAllowedAmount(uint256 amount) {
        require(
            _canTransfer(msg.sender, amount),
            "Cannot transfer more than allowed amount"
        );
        _;
    }

    constructor() {
        _owner = msg.sender;
        _totalSupply = 100000000 * 10**uint256(decimals);
        _totalSupplyAvailable = _totalSupply;
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount)
        external
        override
        onlyAllowedAmount(amount)
        returns (bool)
    {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender)
        external
        view
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external override onlyAllowedAmount(amount) returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            msg.sender,
            _allowances[sender][msg.sender] - amount
        );
        return true;
    }

    function renounceOwnership() external onlyOwner {
        _owner = address(0);
    }

    function _canTransfer(address account, uint256 amount)
        private
        view
        returns (bool)
    {
        if (
            account == _owner ||
            _balances[account] + amount <= (_totalSupply * 4 / 100)
        ) {
            return true;
        }

        uint256 currentTime = block.timestamp;
        uint256 lastTransactionTime = _lastTransactionTime[account];
        return
            currentTime - lastTransactionTime >= 24 hours ||
            amount <= (_totalSupply * 4 / 100);
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) private {
        require(
            sender != address(0),
            "BEP20: transfer from the zero address"
        );
        require(
            recipient != address(0),
            "BEP20: transfer to the zero address"
        );

        uint256 taxAmount = amount * _taxPercentage / 100;
        uint256 transferAmount = amount - taxAmount;

        _balances[sender] -= amount;
        _balances[recipient] += transferAmount;
        _balances[_taxWallet] += taxAmount;
        _totalSupplyAvailable -= taxAmount;

        _lastTransactionTime[sender] = block.timestamp;
        emit Transfer(sender, recipient, transferAmount);
        emit Transfer(sender, _taxWallet, taxAmount);
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) private {
        require(
            owner != address(0),
            "BEP20: approve from the zero address"
        );
        require(
            spender != address(0),
            "BEP20: approve to the zero address"
        );

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}