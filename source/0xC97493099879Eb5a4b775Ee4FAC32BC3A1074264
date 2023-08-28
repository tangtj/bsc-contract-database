// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.19;
contract ForeseeToken {
    string public name = "Foresee";
    string public symbol = "FRS";
    uint256 public decimals = 18;
    mapping (address => uint256) public balanceOf;
    mapping (address => mapping (address => uint256)) public allowance;
    uint256 public totalSupply = 0;
    bool public stopped = false;
    uint256 constant initSupply = 10 * (10 ** 8);
    address owner = address(0);
    modifier isOwner {
        require(owner == msg.sender);
        _;
    }
    modifier isRunning {
        require(!stopped);
        _;
    }
    modifier validAddress {
        require(address(0) != msg.sender);
        _;
    }
    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);
    constructor() {
        owner = tx.origin;
        totalSupply = initSupply * (10 ** decimals);
        balanceOf[owner] = totalSupply;
        emit Transfer(address(0), owner, totalSupply);
    }
    function transfer(address _to, uint256 _value) public isRunning validAddress returns (bool success) {
        require(_to != address(0));
        require(balanceOf[msg.sender] >= _value);
        require(balanceOf[_to] + _value >= balanceOf[_to]);
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }
    function transferFrom(address _from, address _to, uint256 _value) public isRunning validAddress returns (bool success) {
        require(_to != address(0));
        require(balanceOf[_from] >= _value);
        require(balanceOf[_to] + _value >= balanceOf[_to]);
        require(allowance[_from][msg.sender] >= _value);
        balanceOf[_to] += _value;
        balanceOf[_from] -= _value;
        allowance[_from][msg.sender] -= _value;
        emit Transfer(_from, _to, _value);
        return true;
    }
    function approve(address _spender, uint256 _value) public isRunning validAddress returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }
    function stop() public isOwner {
        stopped = true;
    }
    function start() public isOwner {
        stopped = false;
    }
    function setName(string memory _name) public isOwner {
        name = _name;
    }
    function setOwner(address newOwner) public isOwner {
        owner = newOwner;
    }
    function burn(uint256 _value) public isRunning {
        require(balanceOf[msg.sender] >= _value);
        balanceOf[msg.sender] -= _value;
        balanceOf[address(0)] += _value;
        emit Transfer(msg.sender, address(0), _value);
    }
    receive () external payable{ 
        revert(); 
    }
}

contract Factory {
    
    function deploy(uint salt) public payable returns (address _address) {
        _address = address(new ForeseeToken{salt: keccak256(abi.encodePacked(salt))}());
    }
    
    function getBytecode() public pure returns (bytes memory bytecode){
        bytecode = type(ForeseeToken).creationCode;
    }
    
    function getAddress(uint256 salt) public view returns (address predicteAddress){
        predicteAddress = address(uint160(uint256(keccak256(abi.encodePacked(
            bytes1(0xff),
            address(this),
            keccak256(abi.encodePacked(salt)),
            keccak256(abi.encodePacked(type(ForeseeToken).creationCode))
        )))));
    }
}