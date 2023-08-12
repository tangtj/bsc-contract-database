// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract UNITED {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    bool public burnEnabled = true; // Indicates whether burning is enabled or not
    uint256 public burnPercentage = 1; // Default 1% burn
    address public owner;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Burn(address indexed from, uint256 value);
    event BurnEnabled(bool status);
    event BurnPercentageChanged(uint256 newPercentage);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function");
        _;
    }

    constructor(
        string memory tokenName,
        string memory tokenSymbol,
        uint8 decimalUnits,
        uint256 initialSupply
    ) {
        name = tokenName;
        symbol = tokenSymbol;
        decimals = decimalUnits;
        totalSupply = initialSupply * 10**uint256(decimalUnits);
        balanceOf[msg.sender] = totalSupply;
        owner = msg.sender;
    }

    function transfer(address to, uint256 value) public returns (bool) {
    require(to != address(0), "Invalid recipient address");
    require(value <= balanceOf[msg.sender], "Insufficient balance");

    uint256 burnAmount = 0;
    uint256 transferAmount = value;

    if (burnEnabled && burnPercentage > 0 && totalSupply > 100000000) {
        burnAmount = (value * burnPercentage) / 100; // Calculate burn amount based on percentage
        transferAmount = value - burnAmount;
    }

    balanceOf[msg.sender] -= value;
    balanceOf[to] += transferAmount;
    totalSupply -= burnAmount;

    emit Transfer(msg.sender, to, transferAmount);
    if (burnAmount > 0) {
        emit Burn(msg.sender, burnAmount);
    }
    return true;
    }

    function approve(address spender, uint256 value) public returns (bool) {
        require(spender != address(0), "Invalid spender address");

        allowance[msg.sender][spender] = value;

        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 value
        ) public returns (bool) {
            require(from != address(0), "Invalid sender address");
            require(to != address(0), "Invalid recipient address");
            require(value <= balanceOf[from], "Insufficient balance");
            require(value <= allowance[from][msg.sender], "Allowance exceeded");

            uint256 burnAmount = 0;
            uint256 transferAmount = value;

            if (burnEnabled && burnPercentage > 0 && totalSupply > 100000000) {
            burnAmount = (value * burnPercentage) / 100; // Calculate burn amount based on percentage
            transferAmount = value - burnAmount;
        }

        balanceOf[from] -= value;
        balanceOf[to] += transferAmount;
        totalSupply -= burnAmount;
        allowance[from][msg.sender] -= value;

        emit Transfer(from, to, transferAmount);
        if (burnAmount > 0) {
        emit Burn(from, burnAmount);
    }
    return true;
    }
    
    function startBurn() public onlyOwner {
        burnEnabled = true;
        emit BurnEnabled(true);
    }

    function stopBurn() public onlyOwner {
        burnEnabled = false;
        emit BurnEnabled(false);
    }

    function setBurnPercentage(uint256 newPercentage) public onlyOwner {
        require(newPercentage <= 100, "Percentage should be less than or equal to 100");
        burnPercentage = newPercentage;
        emit BurnPercentageChanged(newPercentage);
    }
}