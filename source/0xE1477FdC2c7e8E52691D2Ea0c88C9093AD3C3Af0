// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

contract SecondContract {

    address public admin; // L'adresse de l'administrateur
    address public tokenContractAddress; // L'adresse du contrat de token principal
    mapping(address => uint256) public balances; // Pour suivre les soldes des utilisateurs

    event Transfer(address indexed from, address indexed to, uint256 value); // Événement pour signaler les transferts
    event AdminChanged(address indexed oldAdmin, address indexed newAdmin); // Événement pour signaler le changement d'administrateur

    constructor(address _tokenContractAddress) {
        admin = msg.sender; // Définir l'administrateur comme le créateur du contrat
        tokenContractAddress = _tokenContractAddress; // Initialiser l'adresse du contrat de token principal
    }

    modifier onlyAdmin() {
        require(msg.sender == admin, "Only admin can call this function");
        _;
    }

    modifier onlyTokenContract() {
        require(msg.sender == tokenContractAddress, "Not allowed: only token contract can call this function");
        _;
    }

    // Fonction pour changer l'administrateur
    function changeAdmin(address newAdmin) external onlyAdmin {
        require(newAdmin != address(0), "New admin cannot be the zero address");
        emit AdminChanged(admin, newAdmin);
        admin = newAdmin;
    }

    // Fonction pour permettre à l'administrateur de changer l'adresse du contrat de token principal
    function setTokenContractAddress(address _newTokenContractAddress) external onlyAdmin {
        tokenContractAddress = _newTokenContractAddress;
    }

    // Fonction pour retirer des tokens du contrat
    function withdrawTokens(address to, uint256 amount) external onlyAdmin {
        require(balances[address(this)] >= amount, "Insufficient contract balance");
        balances[address(this)] -= amount;
        balances[to] += amount;

        emit Transfer(address(this), to, amount); // Émettre un événement de transfert
    }

    // Fonction pour gérer le transfert intermédiaire des tokens
    function intermediateTransfer(address from, address to, uint256 amount) external onlyTokenContract {
        require(to != address(0), "Cannot transfer to the zero address");
        require(balances[from] >= amount, "Insufficient balance");

        balances[from] -= amount;
        balances[to] += amount;

        emit Transfer(from, to, amount); // Émettre un événement de transfert
    }
}