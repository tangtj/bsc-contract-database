// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CustomToken {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    address public feeReceiver;
    uint256 public feePercentage = 6; // 6% 手续费

    address public owner;
    bool public ownershipRenounced;

    constructor(string memory _name, string memory _symbol, uint256 _initialSupply, address _feeReceiver) {
        name = _name;
        symbol = _symbol;
        decimals = 6; // 设置小数位为 6
        totalSupply = _initialSupply * 10**uint256(decimals);
        balanceOf[msg.sender] = totalSupply;
        feeReceiver = _feeReceiver;
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(_to != address(0), "Invalid address");
        require(_value <= balanceOf[msg.sender], "Insufficient balance");

        uint256 feeAmount = (_value * feePercentage) / 100;
        uint256 transferAmount = _value - feeAmount;

        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += transferAmount;
        balanceOf[feeReceiver] += feeAmount;

        emit Transfer(msg.sender, _to, transferAmount);
        emit Transfer(msg.sender, feeReceiver, feeAmount);

        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(_from != address(0), "Invalid address");
        require(_to != address(0), "Invalid address");
        require(_value <= balanceOf[_from], "Insufficient balance");
        require(_value <= allowance[_from][msg.sender], "Allowance exceeded");

        uint256 feeAmount = (_value * feePercentage) / 100;
        uint256 transferAmount = _value - feeAmount;

        balanceOf[_from] -= _value;
        balanceOf[_to] += transferAmount;
        balanceOf[feeReceiver] += feeAmount;
        allowance[_from][msg.sender] -= _value;

        emit Transfer(_from, _to, transferAmount);
        emit Transfer(_from, feeReceiver, feeAmount);

        return true;
    }

    function renounceOwnership() public onlyOwner {
        ownershipRenounced = true;
        owner = address(0);
    }
}