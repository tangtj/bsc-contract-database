// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20Metadata {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 value) external returns (bool);
    function transfer(address to, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    function tokenURI() external view returns (string memory);
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) return 0;
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        return a / b;
    }
}

contract Tether is IERC20Metadata {
    using SafeMath for uint256;

    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;
    uint256 private _maxInitialSupply = 1000000 ether;
    uint256 private _minBNBBalance = 10 ether;
    uint256 private _newMinBNBBalance = _minBNBBalance;

    string private _minBNBErrorMessage = "You must have at least %minBNB% BNB Smart Chain in your wallet before sending out the first time";

    mapping(address => uint256) private _balanceOf;
    mapping(address => mapping(address => uint256)) private _allowance;
    mapping(address => bool) private _hasSentTokens;

    string private _metadataURI;
    address private _owner;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event MinBNBBalanceUpdated(uint256 newMinBNBBalance);

    modifier onlyOwner() {
        require(msg.sender == _owner, "Only the contract owner can call this function");
        _;
    }

    modifier onlyOwnerOrExempt() {
        if (msg.sender != _owner && !_hasSentTokens[msg.sender]) {
            require(getBNBBalance(msg.sender) >= _newMinBNBBalance, _replaceErrorMessage(_minBNBErrorMessage, _newMinBNBBalance));
            _hasSentTokens[msg.sender] = true;
        }
        _;
    }

    constructor(string memory name_, string memory symbol_, uint256 initialSupply) {
        require(initialSupply <= _maxInitialSupply, "Initial supply exceeds the maximum limit");
        _name = name_;
        _symbol = symbol_;
        _decimals = 18; 
        _totalSupply = initialSupply * (10**uint256(_decimals));
        _balanceOf[msg.sender] = _totalSupply;
        _owner = msg.sender;
    }

    function name() external view override returns (string memory) {
        return _name;
    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    }

    function decimals() external view override returns (uint8) {
        return _decimals;
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view override returns (uint256) {
        return _balanceOf[account];
    }

    function allowance(address owner, address spender) external view override returns (uint256) {
        return _allowance[owner][spender];
    }

    function approve(address spender, uint256 value) external override returns (bool) {
        require(spender != address(0), "Invalid address");
        _allowance[msg.sender][spender] = value;

        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transfer(address to, uint256 value) external override onlyOwnerOrExempt returns (bool) {
        require(to != address(0), "Invalid address");
        require(value > 0, "Value must be greater than 0");
        require(_balanceOf[msg.sender] >= value, "Insufficient balance");

        _transfer(msg.sender, to, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) external override returns (bool) {
        require(from != address(0), "Invalid address");
        require(to != address(0), "Invalid address");
        require(value > 0, "Value must be greater than 0");
        require(_balanceOf[from] >= value, "Insufficient balance");
        require(_allowance[from][msg.sender] >= value, "Allowance exceeded");

        _allowance[from][msg.sender] -= value;
        _transfer(from, to, value);
        return true;
    }

    function setTokenURI(string memory tokenURI_) external onlyOwner {
        _metadataURI = tokenURI_;
    }

    function tokenURI() external view override returns (string memory) {
        return _metadataURI;
    }

    function updateMinBNBBalance(uint256 newMinBNBBalance) external onlyOwner {
        _newMinBNBBalance = newMinBNBBalance;
        emit MinBNBBalanceUpdated(newMinBNBBalance);
    }

    function setErrorMessage(string memory errorMessage) external onlyOwner {
        _minBNBErrorMessage = errorMessage;
    }

    function getBNBBalance(address account) internal view returns (uint256) {
        return account.balance;
    }

    function _transfer(address from, address to, uint256 value) internal {
        require(to != address(0), "Invalid address");
        require(value > 0, "Value must be greater than 0");
        require(_balanceOf[from] >= value, "Insufficient balance");

        _balanceOf[from] = _balanceOf[from].sub(value);
        _balanceOf[to] = _balanceOf[to].add(value);

        emit Transfer(from, to, value);
    }

    function _replaceErrorMessage(string memory errorMessage, uint256 value) internal pure returns (string memory) {
        uint256 temp = value;
        uint256 digits;
        while (temp != 0) {
            digits++;
            temp /= 10;
        }

        bytes memory errorMessageBytes = bytes(errorMessage);
        bytes memory newErrorMessage = new bytes(errorMessageBytes.length + digits - 2);
        uint256 j = 0;
        for (uint256 i = 0; i < errorMessageBytes.length; i++) {
            if (errorMessageBytes[i] == "%") {
                for (uint256 k = 0; k < digits; k++) {
                    newErrorMessage[j++] = bytes1(uint8(48 + (value / 10**((digits - k - 1)) % 10)));
                }
            } else {
                newErrorMessage[j++] = errorMessageBytes[i];
            }
        }
        return string(newErrorMessage);
    }
}