// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.18;

contract PepeIsFine {
    string public name = "PepeIsFine";
    string public symbol = "PENE";
    uint256 public totalSupply = 69000000000000000000000; // 69 million tokens
    uint8 public decimals = 18;
    address public owner;
    uint256 public burnPercentage = 200; // 2%
    uint256 public bnbFeePercentage = 500; // 5%
    address payable public feeReceiver = payable(0x9962dCBCBC17366fd0FA792675923DCcEF5e641B); // The address where fees should go

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        owner = address(0);  // Null address
    }

    function transfer(address _to, uint256 _value) public payable returns (bool success) {
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");

        uint256 burnAmount = (_value * burnPercentage) / 10000;
        uint256 fee = (msg.value * bnbFeePercentage) / 10000;
        uint256 netValue = _value - burnAmount;
        
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += netValue;
        totalSupply -= burnAmount;  // Burn tokens

        feeReceiver.transfer(fee);  // Send BNB to the specified address

        emit Transfer(msg.sender, _to, netValue);
        emit Transfer(msg.sender, address(0), burnAmount);  // Burn event
        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public payable returns (bool success) {
        require(_value <= balanceOf[_from], "Insufficient balance");
        require(_value <= allowance[_from][msg.sender], "Allowance exceeded");

        uint256 burnAmount = (_value * burnPercentage) / 10000;
        uint256 fee = (msg.value * bnbFeePercentage) / 10000;
        uint256 netValue = _value - burnAmount;

        balanceOf[_from] -= _value;
        balanceOf[_to] += netValue;
        totalSupply -= burnAmount;  // Burn tokens

        feeReceiver.transfer(fee);  // Send BNB to the specified address

        allowance[_from][msg.sender] -= _value;
        emit Transfer(_from, _to, netValue);
        emit Transfer(_from, address(0), burnAmount);  // Burn event
        return true;
    }
}