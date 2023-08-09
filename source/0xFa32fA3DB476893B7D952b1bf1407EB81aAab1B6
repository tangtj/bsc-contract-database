// SPDX-License-Identifier: MIT

pragma solidity 0.8.4;

contract PEPEX {
    string public name = "PEPE X";
    string public symbol = "PEPEX";
    uint256 public totalSupply = 420000000000000000000000000000000;
    uint8 public decimals = 18;

    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);
    event OwnershipRenounced(address indexed previousOwner);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    address private contractOwner;

    mapping(address => uint256) public saleRequestTime;

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        contractOwner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == contractOwner, "");
        _;
    }

    modifier onlyAfterOneDay(address _address) {
        require(block.timestamp >= saleRequestTime[_address] + 1 days, "");
        _;
    }

    function requestSale() public {
        saleRequestTime[msg.sender] = block.timestamp;
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
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

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
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
}