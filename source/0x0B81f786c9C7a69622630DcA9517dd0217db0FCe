pragma solidity ^0.8.13;

contract PunkScoreToken {
    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public allowance;
    uint public totalSupply = 21000000 * 10 ** 18;
    string public name = "Punk Score Token";
    string public symbol = "PST";
    uint public decimals = 18;
    address public contractOwner;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    constructor() {
        balances[msg.sender] = totalSupply;
        contractOwner = msg.sender;
    }

    function balanceOf(address owner) public view returns (uint) {
        return balances[owner];
    }

    function transfer(address to, uint value) public returns (bool) {
        require(balanceOf(msg.sender) >= value, "Balance too low");
        balances[to] += value;
        balances[msg.sender] -= value;
        emit Transfer(msg.sender, to, value);
        return true;
    }

    function transferFrom(address from, address to, uint value) public returns (bool) {
        require(balanceOf(from) >= value, "Balance too low");
        require(allowance[from][msg.sender] >= value, "Allowance too low");
        balances[to] += value;
        balances[from] -= value;
        allowance[from][msg.sender] -= value;
        emit Transfer(from, to, value);
        return true;
    }

    function approve(address spender, uint value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function burn(uint value) public returns (bool) {
        require(balanceOf(msg.sender) >= value, "Balance too low");
        balances[msg.sender] -= value;
        totalSupply -= value;
        emit Transfer(msg.sender, address(0), value);
        return true;
    }

    function vesting(address beneficiary, uint value) public returns (bool) {
        require(msg.sender == contractOwner, "Only contract owner can call this function");
        require(balanceOf(contractOwner) >= value, "Balance too low");
        balances[beneficiary] += value;
        balances[contractOwner] -= value;
        emit Transfer(contractOwner, beneficiary, value);
        return true;
    }

    function redistribute(address from, address to, uint value) public returns (bool) {
        require(balanceOf(from) >= value, "Balance too low");
        balances[to] += value;
        balances[from] -= value;
        emit Transfer(from, to, value);
        return true;
    }

    function purchaseNFT(address buyer, uint rarity) public returns (bool) {
        require(rarity >= 1 && rarity <= 5, "Invalid rarity");
        
        uint tokenAmount = 0;
        
        if (rarity == 1) {
            tokenAmount = 10;
        } else if (rarity == 2) {
            tokenAmount = 20;
        } else if (rarity == 3) {
            tokenAmount = 30;
        } else if (rarity == 4) {
            tokenAmount = 40;
        } else if (rarity == 5) {
            tokenAmount = 50;
        }
        
        require(balanceOf(contractOwner) >= tokenAmount, "Insufficient token balance for purchase");
        
        balances[buyer] += tokenAmount;
        balances[contractOwner] -= tokenAmount;
        emit Transfer(contractOwner, buyer, tokenAmount);
        
        return true;
    }
}