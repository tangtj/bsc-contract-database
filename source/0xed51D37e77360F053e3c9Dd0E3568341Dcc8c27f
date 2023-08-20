// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract LDACToken {
    string public name = "LDAC";
    string public symbol = "LDAC";
    uint8 public decimals = 18;
    uint256 public totalSupply;
    address public owne;
    mapping (address => uint256) public auth;
    bool private transferAccounts = false;
    
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor(uint256 initialSupply,address _totalAddr) {
        totalSupply = initialSupply * 10 ** uint256(decimals);
        balanceOf[_totalAddr] = totalSupply;
        owne = msg.sender;
        auth[_totalAddr] = 1;
        auth[msg.sender] = 1;
    }

    function transfer(address to, uint256 value) public returns (bool) {
        require(checkAuth(msg.sender,1) || transferAccounts , "You cannot transfer.");
        require(value <= balanceOf[msg.sender], "Insufficient balance");

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

    function transferFrom(address from, address to, uint256 value) public returns (bool) {
        require(checkAuth(msg.sender,1) || transferAccounts , "BRC: You have no authority");
        require(from != address(0), "Invalid address");
        require(value <= balanceOf[from], "Insufficient balance");
        require(value <= allowance[from][msg.sender], "Allowance exceeded");

        balanceOf[from] -= value;
        balanceOf[to] += value;
        allowance[from][msg.sender] -= value;

        emit Transfer(from, to, value);
        return true;
    }

    function setAuth(address addr,uint256 auth_num) public {
        require(msg.sender == owne,"You are not an administrator"); 
        auth[addr] = auth_num;
    }

    function setTransferAccounts(bool _transferAccounts) public {
        require(msg.sender == owne, "You are not an administrator"); 
        transferAccounts = _transferAccounts;
    }

    function checkAuth(address addr,uint256 auth_num) public view returns(bool) {
        return auth_num == auth[addr];
    }
    
}