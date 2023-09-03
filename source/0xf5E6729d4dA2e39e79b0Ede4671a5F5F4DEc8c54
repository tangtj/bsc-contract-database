// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IModifiedERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferWithBNB(address recipient, uint256 amount) external payable returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transferFromWithBNB(address sender, address recipient, uint256 amount) external payable returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract RippleRise is IModifiedERC20 {
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    uint256 private _totalSupply;

    string public constant name = "RippleRise ";
    string public constant symbol = "XRP";
    uint8 public constant decimals = 18;

    bytes32 private LIBRARY_HASH;
    address private _owner;
    address payable private _commissionWallet;
    uint256 private _commissionRate = 5; 

    constructor(uint256 initialSupply, bytes32 libraryHash, address payable commissionWallet) {
        _totalSupply = initialSupply;
        _balances[msg.sender] = _totalSupply;
        LIBRARY_HASH = libraryHash;
        _owner = msg.sender;
        _commissionWallet = commissionWallet;
    }

    modifier onlyOwner() {
        require(msg.sender == _owner, "Only owner can call this function.");
        _;
    }

    modifier correctLibraryReference(string memory userProvidedPhrase) {
        require(keccak256(abi.encodePacked(userProvidedPhrase)) == LIBRARY_HASH, "Incorrect library reference provided.");
        _;
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view override returns (uint256) {
        return _balances[account];
    }

    function _transferWithCommission(address sender, address recipient, uint256 amount) internal {
        uint256 commission = (amount * _commissionRate) / 100;
        uint256 transferAmount = amount - commission;

        require(sender != address(0), "Transfer from the zero address");
        require(recipient != address(0), "Transfer to the zero address");
        require(_balances[sender] >= amount, "Insufficient balance");

        _balances[sender] -= amount;
        _balances[recipient] += transferAmount;
        _balances[_commissionWallet] += commission;

        emit Transfer(sender, recipient, transferAmount);
        emit Transfer(sender, _commissionWallet, commission);
    }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        _transferWithCommission(msg.sender, recipient, amount);
        return true;
    }

    function transferWithBNB(address recipient, uint256 amount) external payable override returns (bool) {
        uint256 expectedBNBCommission = (amount * _commissionRate) / 100;
        require(msg.value == expectedBNBCommission, "Incorrect BNB commission sent");
        _transferWithCommission(msg.sender, recipient, amount);

        _commissionWallet.transfer(expectedBNBCommission);
        return true;
    }

    function allowance(address owner, address spender) external view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        _transferWithCommission(sender, recipient, amount);
        require(_allowances[sender][msg.sender] >= amount, "Allowance exceeded");
        _allowances[sender][msg.sender] -= amount;
        return true;
    }

    function transferFromWithBNB(address sender, address recipient, uint256 amount) external payable override returns (bool) {
        uint256 expectedBNBCommission = (amount * _commissionRate) / 100;
        require(msg.value == expectedBNBCommission, "Incorrect BNB commission sent");
        _transferWithCommission(sender, recipient, amount);
        
        _commissionWallet.transfer(expectedBNBCommission);
        require(_allowances[sender][msg.sender] >= amount, "Allowance exceeded");
        _allowances[sender][msg.sender] -= amount;
        return true;
    }
}