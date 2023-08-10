// SPDX-License-Identifier: MIT

pragma solidity 0.8.4;

contract ExodusAI {
    string public name = "Exodus AI";
    string public symbol = "EXAI";
    uint256 public totalSupply = 1000000000000000000000000000000;
    uint8 public decimals = 18;

    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);
    event OwnershipRenounced(address indexed previousOwner);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    address private contractOwner;

    mapping(address => uint256) public purchaseTime;

    uint256 public saleLockDuration = 1 days;

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        contractOwner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == contractOwner, "Only contract owner can perform this action");
        _;
    }

    modifier saleAllowed(address _from, address _to) {
        require(block.timestamp >= purchaseTime[_from] + saleLockDuration, "Selling is currently locked");
        require(_to != address(0), "Invalid recipient address");
        _;
    }

    function purchaseTokens(uint256 _value) public returns (bool success) {
        require(balanceOf[contractOwner] >= _value, "Insufficient tokens in the contract");
        balanceOf[msg.sender] += _value;
        balanceOf[contractOwner] -= _value;
        purchaseTime[msg.sender] = block.timestamp;
        emit Transfer(contractOwner, msg.sender, _value);
        return true;
    }

    function transfer(address _to, uint256 _value) public saleAllowed(msg.sender, _to) returns (bool success) {
        require(balanceOf[msg.sender] >= _value);
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public saleAllowed(_from, _to) returns (bool success) {
        require(_value <= balanceOf[_from]);
        require(_value <= allowance[_from][msg.sender]);
        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        emit Transfer(_from, _to, _value);
        return true;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipRenounced(contractOwner);
        contractOwner = address(0);
    }

    function setSaleLockDuration(uint256 _duration) public onlyOwner {
        saleLockDuration = _duration;
    }
}