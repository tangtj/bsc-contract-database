// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.15; 

contract VaquitaToken{
    string public name = "Vaquita Token";
    string public symbol = "VQT";
    uint256 public totalSupply;
    uint8 public decimals = 8; 

    mapping(address => uint256) balances; 
    mapping(address => mapping(address => uint256)) allowances;

    // Eventos
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function balanceOf(address account) public view returns (uint256){
        return balances[account]; 
    }

    // Constructor
    constructor(uint256 _totalSupply, address initialAccount){
        totalSupply = _totalSupply;
        balances[initialAccount] = _totalSupply; 

        emit Transfer(address(0), initialAccount, _totalSupply);

    }

    // Funcion para transferencias
    function transfer(address recipent, uint256 amount)public{
        require(balances[msg.sender] >= amount,"insufficient balance"); 
        balances[msg.sender] -= amount;
        balances[recipent] += amount; 

        emit Transfer(msg.sender, recipent, amount);
    }
    // Funcion para dar permisos
    function allowance(address owner, address spender)public view returns(uint256){
        return allowances[owner][spender];
    } 

    // Funcion de aprobacion
    function approve(address spender, uint256 amount)public {
        require(spender != address(0), "Aprrove to zero address");
        allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
    }
    // Funcion transferForm para enviar desde otro usuario o contrato

    function transferFrom(address sender, address recipient, uint256 amount) public {
        require(balances[sender] >= amount,"Insufficient balance"); 
        require(allowances[sender][msg.sender] >= amount, "Insufficient allowance"); 
        balances[sender] -= amount;
        balances[recipient] += amount;
        allowances[sender][msg.sender] -= amount;
        
        emit Transfer (sender, recipient, amount);
    }

    }