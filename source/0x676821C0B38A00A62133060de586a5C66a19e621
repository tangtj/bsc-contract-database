// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SEIDaoToken {
    string public constant name = "SEI DAO";
    string public constant symbol = "SEID";
    string public constant description = "SEI DAO token";
    uint8 public constant decimals = 18;
    uint256 public constant totalSupply = 1000000 * (10 ** uint256(decimals));

    address public constant taxWallet = 0x307AE01D945fB551549645B8432F44EA7793eB1B;
    uint8 public constant taxRate = 5;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        balanceOf[msg.sender] = totalSupply;
    }

    function transfer(address _to, uint256 _value) public returns (bool) {
        require(_to != address(0), "Invalid address");
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");

        uint256 taxAmount = (_value * taxRate) / 100;
        uint256 transferAmount = _value - taxAmount;

        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += transferAmount;
        balanceOf[taxWallet] += taxAmount;

        emit Transfer(msg.sender, _to, transferAmount);
        emit Transfer(msg.sender, taxWallet, taxAmount);

        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool) {
        require(_spender != address(0), "Invalid address");
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool) {
        require(_from != address(0), "Invalid address");
        require(_to != address(0), "Invalid address");
        require(balanceOf[_from] >= _value, "Insufficient balance");
        require(allowance[_from][msg.sender] >= _value, "Allowance exceeded");

        uint256 taxAmount = (_value * taxRate) / 100;
        uint256 transferAmount = _value - taxAmount;

        balanceOf[_from] -= _value;
        balanceOf[_to] += transferAmount;
        balanceOf[taxWallet] += taxAmount;
        allowance[_from][msg.sender] -= _value;

        emit Transfer(_from, _to, transferAmount);
        emit Transfer(_from, taxWallet, taxAmount);

        return true;
    }
}