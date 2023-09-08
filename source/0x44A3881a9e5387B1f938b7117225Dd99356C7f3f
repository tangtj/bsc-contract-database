// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SmartDoge {
    string public name = "Smart Doge";
    string public symbol = "SDOGE";
    uint8 public decimals = 18;
    uint256 public totalSupply = 1e9 * 1e18; // 1 bilhão de tokens com 18 casas decimais

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(address => bool) public isBlocked;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Blocked(address indexed wallet, bool isBlocked);

    address public owner;

    constructor() {
        owner = msg.sender;
        balanceOf[msg.sender] = totalSupply;
    }

    modifier onlyOwner() {
        require(
            msg.sender == owner,
            "Somente o proprietario pode executar esta acao."
        );
        _;
    }

    modifier canTransfer(address sender, address recipient) {
        require(
            !isBlocked[sender] && !isBlocked[recipient],
            "Uma ou ambas as carteiras estao bloqueadas."
        );
        _;
    }

    function transfer(address to, uint256 value)
        public
        canTransfer(msg.sender, to)
        returns (bool)
    {
        require(to != address(0), "Endereco invalido.");
        require(balanceOf[msg.sender] >= value, "Saldo insuficiente.");

        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;

        emit Transfer(msg.sender, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) public canTransfer(from, to) returns (bool) {
        require(to != address(0), "Endereco invalido.");
        require(balanceOf[from] >= value, "Saldo insuficiente.");
        require(
            allowance[from][msg.sender] >= value,
            "Permissao insuficiente."
        );

        balanceOf[from] -= value;
        balanceOf[to] += value;
        allowance[from][msg.sender] -= value;

        emit Transfer(from, to, value);
        return true;
    }

    function blockAddress(address wallet, bool isBlockedValue)
        external
        onlyOwner
    {
        isBlocked[wallet] = isBlockedValue;
        emit Blocked(wallet, isBlockedValue);
    }
}