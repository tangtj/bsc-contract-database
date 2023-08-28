// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract OZONToken {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(address => bool) public whitelist;
    address public admin;
    address public lpAddress;
    address public blackholeAddress;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor(string memory _name, string memory _symbol, uint8 _decimals, uint256 _totalSupply) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _totalSupply;
        balanceOf[msg.sender] = _totalSupply;
        admin = msg.sender;
    }

    modifier onlyAdmin() {
        require(msg.sender == admin, "Only admin can call this function.");
        _;
    }

    function setLPAddress(address _lpAddress) external onlyAdmin {
        lpAddress = _lpAddress;
    }

    function setAdmin(address _newAdmin) external onlyAdmin {
        admin = _newAdmin;
    }

    function addToWhitelist(address _address) external onlyAdmin {
        whitelist[_address] = true;
    }

    function removeFromWhitelist(address _address) external onlyAdmin {
        whitelist[_address] = false;
    }

    function setBlackholeAddress(address _blackholeAddress) external onlyAdmin {
        blackholeAddress = _blackholeAddress;
    }

    function transfer(address _to, uint256 _value) external returns (bool success) {
        _transfer(msg.sender, _to, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) external returns (bool success) {
        require(_value <= allowance[_from][msg.sender], "Insufficient allowance");
        allowance[_from][msg.sender] -= _value;
        _transfer(_from, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value) external returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function _transfer(address _from, address _to, uint256 _value) internal {
        require(_to != address(0), "Invalid recipient");
        require(balanceOf[_from] >= _value, "Insufficient balance");

        if (_from != admin && _to == lpAddress && !whitelist[_from]) {
            // Sell transaction
            uint256 fee = _value * 5 / 100;
            uint256 amount = _value - fee;

            balanceOf[_from] -= _value;
            balanceOf[_to] += amount;
            balanceOf[blackholeAddress] += fee;

            emit Transfer(_from, _to, amount);
            emit Transfer(_from, blackholeAddress, fee);
        } else {
            // Regular transaction
            balanceOf[_from] -= _value;
            balanceOf[_to] += _value;

            emit Transfer(_from, _to, _value);
        }
    }
}