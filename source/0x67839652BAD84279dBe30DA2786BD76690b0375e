// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CourseraToken {
    string public constant name = "Coursera";
    string public constant symbol = "CSRA";
    uint8 public constant decimals = 18;
    uint256 private constant _totalSupply = 1_000_000_000_000 * 10**uint256(decimals); // Total supply: 1 trillion
    
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    
    address public feeAddress = 0xfd70b14CA8F392d873c25Aee0da9B1137D431646;
    uint256 public feePercentage = 2;

    constructor() {
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external pure returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) external returns (bool) {
        uint256 feeAmount = (amount * feePercentage) / 100;
        uint256 netAmount = amount - feeAmount;
        
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(_balances[msg.sender] >= amount, "BEP20: insufficient balance");
        
        _balances[msg.sender] -= amount;
        _balances[recipient] += netAmount;
        _balances[feeAddress] += feeAmount;
        
        emit Transfer(msg.sender, recipient, netAmount);
        emit Transfer(msg.sender, feeAddress, feeAmount);
        
        return true;
    }

    function allowance(address owner, address spender) external view returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool) {
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(_balances[sender] >= amount, "BEP20: insufficient balance");
        require(_allowances[sender][msg.sender] >= amount, "BEP20: allowance exceeded");
        
        uint256 feeAmount = (amount * feePercentage) / 100;
        uint256 netAmount = amount - feeAmount;
        
        _balances[sender] -= amount;
        _balances[recipient] += netAmount;
        _balances[feeAddress] += feeAmount;
        _allowances[sender][msg.sender] -= amount;
        
        emit Transfer(sender, recipient, netAmount);
        emit Transfer(sender, feeAddress, feeAmount);
        
        return true;
    }

    // Additional function to set fee address
    function setFeeAddress(address _newFeeAddress) external {
        require(msg.sender == feeAddress, "Only the current fee address can change it");
        feeAddress = _newFeeAddress;
    }

    // Additional function to set fee percentage
    function setFeePercentage(uint256 _newFeePercentage) external {
        require(msg.sender == feeAddress, "Only the current fee address can change it");
        require(_newFeePercentage <= 10, "Fee percentage can't exceed 10%");
        feePercentage = _newFeePercentage;
    }
}