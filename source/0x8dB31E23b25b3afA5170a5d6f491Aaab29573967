// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract OrderRegistry {
    address public owner;
    uint256 public orderCount;
    
    struct Order {
        address walletAddress;
        string encryptedValue;
    }
    
    mapping(uint256 => Order) public orders;
    
    event OrderRecorded(uint256 indexed orderID, address indexed walletAddress, string encryptedValue);
    
    constructor() {
        owner = msg.sender;
        orderCount = 0;
    }
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function");
        _;
    }
    
    function recordOrder(string memory encryptedValue) public returns (uint256) {
        require(bytes(encryptedValue).length > 0, "Encrypted value cannot be empty");
        
        orderCount++;
        orders[orderCount] = Order(msg.sender, encryptedValue);
        
        emit OrderRecorded(orderCount, msg.sender, encryptedValue);
        
        return orderCount;
    }
    
    function getEncryptedValue(uint256 orderID) public view returns (string memory) {
        require(orderID > 0 && orderID <= orderCount, "Invalid order ID");
        
        return orders[orderID].encryptedValue;
    }
}