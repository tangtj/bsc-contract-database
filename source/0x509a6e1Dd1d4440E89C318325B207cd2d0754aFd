// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CoincapitaProToken {
    string public name = "CoincapitaPro Token";
    string public symbol = "CCP";
    uint8 public decimals = 18;
    uint256 public totalSupply = 1_000_000 * 10**uint256(decimals);
    address public owner;
    address public feeRecipient = 0x12dC584B9D6c8558Aa8C86Ef01f070982c407Fe0; // Dirección para recibir las comisiones

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    
    uint256 public feePercent = 3;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        owner = msg.sender;
        _balances[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }

    function balanceOf(address account) external view returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) external returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function allowance(address tokenOwner, address spender) external view returns (uint256) {
        return _allowances[tokenOwner][spender];
    }

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender] - amount);
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "CCP: transfer from the zero address");
        require(recipient != address(0), "CCP: transfer to the zero address");
        require(amount > 0, "CCP: transfer amount must be greater than zero");

        uint256 feeAmount = (amount * feePercent) / 100;
        uint256 netAmount = amount - feeAmount;

        _balances[sender] -= amount;
        _balances[recipient] += netAmount;

        // Envía la tarifa a la dirección de comisiones
        _balances[feeRecipient] += feeAmount;

        emit Transfer(sender, recipient, netAmount);
        emit Transfer(sender, feeRecipient, feeAmount);
    }

    function _approve(address tokenOwner, address spender, uint256 amount) internal {
        require(tokenOwner != address(0), "CCP: approve from the zero address");
        require(spender != address(0), "CCP: approve to the zero address");

        _allowances[tokenOwner][spender] = amount;
        emit Approval(tokenOwner, spender, amount);
    }

    // Función para renunciar a la propiedad del contrato
    function renounceOwnership() external {
        require(msg.sender == owner, "CCP: Only the owner can renounce ownership");
        owner = address(0);
    }
}