// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

    contract StelarBank {
    string public name = "STELARBANK";
    string public symbol = "STBK";
    uint8 public decimals = 18;
    uint256 public totalSupply = 100000000000000000000000000000;
    uint256 public burnRate = 1; // 1% de tasa de deflación
    bool public paused = false;
    address public owner;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    modifier onlyOwner() {
        require(msg.sender == owner, "Usted no es el propietario");
        _;
    }

    modifier whenNotPaused() {
       require(!paused, "Transfers of tokens are paused");


        _;
    }

    constructor() {
        owner = msg.sender;
        balanceOf[msg.sender] = totalSupply;
    }

    function transfer(address _to, uint256 _value) public whenNotPaused returns (bool) {
        require(_to != address(0) && _to != owner, "Only the owner or the recipient can transfer");
        uint256 burnAmount = (_value * burnRate) / 100;
        uint256 sendAmount = _value - burnAmount;
        require(balanceOf[msg.sender] >= _value, "Saldo insuficiente");
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += sendAmount;
        totalSupply -= burnAmount;
        emit Transfer(msg.sender, _to, sendAmount);
        emit Transfer(msg.sender, address(0), burnAmount);
        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public whenNotPaused returns (bool) {
        require(_from != address(0) && _to != address(0), "Invalid address");
        uint256 burnAmount = (_value * burnRate) / 100;
        uint256 sendAmount = _value - burnAmount;
        require(_value <= balanceOf[_from], "Saldo insuficiente");
        require(_value <= allowance[_from][msg.sender], "Permiso excedido");
        balanceOf[_from] -= _value;
        balanceOf[_to] += sendAmount;
        totalSupply -= burnAmount;
        allowance[_from][msg.sender] -= _value;
        emit Transfer(_from, _to, sendAmount);
        emit Transfer(_from, address(0), burnAmount);
        return true;
    }

    function pause() public onlyOwner {
        paused = true;
    }

    function unpause() public onlyOwner {
        paused = false;
    }

    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);
}