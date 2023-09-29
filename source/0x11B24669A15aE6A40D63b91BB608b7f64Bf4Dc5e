// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

// ERC20 standard interface.
interface IERC20v2 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address who) external view returns (uint256);
    function transfer(address to, uint256 value) external returns (bool);
    function allowance(address owner, address delegate) external view returns (uint256);
    function approve(address delegate, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed delegate, uint256 value);
}

// Base contract for managing ownership.
abstract contract OwnerManager {
    address private _contractOwner;
    event OwnershipShifted(address indexed previousOwner, address indexed newOwner);
    
    constructor() {
        _changeOwner(msg.sender);
    }

    function owner() public view returns (address) {
        return _contractOwner;
    }

    modifier onlyOwner() {
        require(owner() == msg.sender, "OwnerManager: caller isn't the owner");
        _;
    }

    function renounceOwnership() public onlyOwner {
        _changeOwner(address(0));
    }

    function transferOwnership(address freshOwner) public onlyOwner {
        require(freshOwner != address(0), "OwnerManager: new owner is the zero address");
        _changeOwner(freshOwner);
    }

    function _changeOwner(address freshOwner) private {
        address oldOwner = _contractOwner;
        _contractOwner = freshOwner;
        emit OwnershipShifted(oldOwner, freshOwner);
    }
}

// Base token contract to emit event after token creation.
abstract contract BasicTokenTemplate {
    event TokenGenerated(address indexed owner, address indexed token, TokenType tokenType, uint256 version);
    enum TokenType { basic }
}

// Standard ERC20 token with ownership features.
contract AdvancedTokenSystem is IERC20v2, OwnerManager, BasicTokenTemplate {
    
    uint256 public constant RELEASE_VERSION = 1;

    mapping(address => uint256) private _walletBalances;
    mapping(address => mapping(address => uint256)) private _walletAllowances;

    string private _tokenTitle;
    string private _tokenAbbreviation;
    uint8 private _decimalUnits;
    uint256 private _maxSupply;

    constructor(string memory title, string memory abbreviation, uint8 decimalUnits, uint256 maxSupply) {
    _tokenTitle = title;
    _tokenAbbreviation = abbreviation;
    _decimalUnits = decimalUnits;
    _walletBalances[msg.sender] = maxSupply;
    _maxSupply = maxSupply;
    emit TokenGenerated(msg.sender, address(this), TokenType.basic, RELEASE_VERSION);

    // Emit a transfer event from the zero address to the owner for the max supply
    emit Transfer(address(0), msg.sender, maxSupply);
}

    function name() public view returns (string memory) {
        return _tokenTitle;
    }

    function symbol() public view returns (string memory) {
        return _tokenAbbreviation;
    }

    function decimals() public view returns (uint8) {
        return _decimalUnits;
    }

    function totalSupply() public view override returns (uint256) {
        return _maxSupply;
    }

    function balanceOf(address who) public view override returns (uint256) {
        return _walletBalances[who];
    }

    function transfer(address to, uint256 value) public override returns (bool) {
        _makeTransfer(msg.sender, to, value);
        return true;
    }

    function allowance(address owner, address delegate) public view override returns (uint256) {
        return _walletAllowances[owner][delegate];
    }

    function approve(address delegate, uint256 amount) public override returns (bool) {
        _setApproval(msg.sender, delegate, amount);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) public override returns (bool) {
        _makeTransfer(from, to, value);
        _modifyAllowance(from, msg.sender, value);
        return true;
    }

    function increaseAllowance(address delegate, uint256 addedValue) public returns (bool) {
        _setApproval(msg.sender, delegate, _walletAllowances[msg.sender][delegate] + addedValue);
        return true;
    }

    function decreaseAllowance(address delegate, uint256 subtractedValue) public returns (bool) {
        uint256 presentAllowance = _walletAllowances[msg.sender][delegate];
        require(presentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        _setApproval(msg.sender, delegate, presentAllowance - subtractedValue);
        
        return true;
    }

    function _makeTransfer(address from, address to, uint256 value) internal {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        
        uint256 fromBalance = _walletBalances[from];
        require(fromBalance >= value, "ERC20: transfer amount exceeds balance");
        
        _walletBalances[from] = fromBalance - value;
        _walletBalances[to] += value;

        emit Transfer(from, to, value);
    }

    function _setApproval(address owner, address delegate, uint256 amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(delegate != address(0), "ERC20: approve to the zero address");

        _walletAllowances[owner][delegate] = amount;
        emit Approval(owner, delegate, amount);
    }

    function _modifyAllowance(address from, address executor, uint256 value) internal {
        uint256 presentAllowance = _walletAllowances[from][executor];
        require(presentAllowance >= value, "ERC20: transfer amount exceeds allowance");
        _setApproval(from, executor, presentAllowance - value);
    }
}