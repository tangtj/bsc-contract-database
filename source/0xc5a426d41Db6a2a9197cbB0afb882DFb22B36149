// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

contract VirtualCity {
    // Объявление переменных и структур
    address public owner;
    uint256 public cityBalance;
    uint256 public citizenCount;
    uint256 public buildingCount;

    struct Citizen {
        address citizenAddress;
        uint256 balance;
        uint256[] ownedProperties;
    }
    mapping(address => Citizen) public citizens;

    struct Building {
        uint256 buildingId;
        string name;
        uint256 value;
        address owner;
    }
    mapping(uint256 => Building) public buildings;

    // Объявление событий
    event NewCitizen(address indexed citizen);
    event NewBuilding(uint256 indexed buildingId, string name, uint256 value, address indexed owner);
    event TransferBuilding(uint256 indexed buildingId, address indexed previousOwner, address indexed newOwner);
    event Deposit(address indexed depositor, uint256 amount);
    event Withdraw(address indexed withdrawer, uint256 amount);

    // Конструктор контракта
    constructor() {
        owner = msg.sender;
    }

    // Функция для создания нового горожанина
    function createCitizen() public {
        require(citizens[msg.sender].citizenAddress == address(0), "Citizen already exists");
        citizens[msg.sender].citizenAddress = msg.sender;
        citizens[msg.sender].balance = 0;
        citizenCount++;
        emit NewCitizen(msg.sender);
    }

    // Функция для создания нового здания
    function createBuilding(string memory name, uint256 value) public {
        buildingCount++;
        buildings[buildingCount] = Building(buildingCount, name, value, address(0));
        emit NewBuilding(buildingCount, name, value, address(0));
    }

    // Функция для покупки здания
    function buyBuilding(uint256 buildingId) public payable {
        require(buildings[buildingId].buildingId != 0, "Building does not exist");
        require(msg.value == buildings[buildingId].value, "Incorrect payment amount");
        require(buildings[buildingId].owner == address(0), "Building already owned");

        buildings[buildingId].owner = msg.sender;
        citizens[msg.sender].balance -= msg.value;
        cityBalance += msg.value;

        emit TransferBuilding(buildingId, address(0), msg.sender);
    }

    // Функция для депозита средств на баланс горожанина
    function deposit() public payable {
        citizens[msg.sender].balance += msg.value;
        emit Deposit(msg.sender, msg.value);
    }

    // Функция для вывода средств со счета горожанина
    function withdraw(uint256 amount) public {
        require(citizens[msg.sender].balance >= amount, "Not enough balance");
        citizens[msg.sender].balance -= amount;
        payable(msg.sender).transfer(amount);
        emit Withdraw(msg.sender, amount);
    }
}