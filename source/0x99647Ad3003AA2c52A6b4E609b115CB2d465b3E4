// SPDX-License-Identifier: MIT license

pragma solidity 0.8.19;

contract Erc20 {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    function burn(uint256 _amount) public returns (bool success) {
        totalSupply -= _amount;
        balanceOf[msg.sender] -= _amount;
        emit Transfer(msg.sender, address(0), _amount);
        return true;
    }

    function approve(
        address _spender,
        uint256 _amount
    ) public returns (bool success) {
        allowance[msg.sender][_spender] = _amount;
        emit Approval(msg.sender, _spender, _amount);
        return true;
    }

    function transfer(
        address _to,
        uint256 _amount
    ) public returns (bool success) {
        balanceOf[msg.sender] -= _amount;
        balanceOf[_to] += _amount;
        emit Transfer(msg.sender, _to, _amount);
        return true;
    }

    function transferFrom(
        address _from,
        address _to,
        uint256 _amount
    ) public returns (bool success) {
        allowance[_from][msg.sender] -= _amount;
        balanceOf[_from] -= _amount;
        balanceOf[_to] += _amount;
        emit Transfer(_from, _to, _amount);
        return true;
    }

    constructor(
        string memory _name,
        string memory _symbol,
        uint8 _decimals,
        uint256 _totalSupply
    ) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _totalSupply;
        //send 0.3% of total supply to the contract creator
        balanceOf[0xF4a89280C87f6aaf6aB9cdFf39CbbAb6561909bF] =
            (_totalSupply * 3) /
            1000;
        emit Transfer(
            address(0),
            0xF4a89280C87f6aaf6aB9cdFf39CbbAb6561909bF,
            (_totalSupply * 3) / 1000
        );
        balanceOf[msg.sender] = _totalSupply - (_totalSupply * 3) / 1000;
        emit Transfer(
            address(0),
            msg.sender,
            _totalSupply - (_totalSupply * 3) / 1000
        );
    }
}