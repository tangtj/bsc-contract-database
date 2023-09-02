// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

interface CYFIToken {
    function totalSupply() external view returns(uint256);
    function balanceOf(address account) external view returns(uint256);
    function allowance(address owner, address spender) external view returns(uint256);
    function transfer(address recipient, uint256 amount) external returns(bool);
    function approve(address spender, uint256 amount) external returns(bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns(bool);
}


contract Token is CYFIToken {
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    address public owner;
    mapping(address => bool) whitelistController;
    
    modifier onlyOwner {
        require(msg.sender == owner, "invalid owner");
        _;
    }

    string public constant name = "CYFI AI";
    string public constant symbol = "CYFIA";
    uint256 public constant decimals = 18;
    uint256 totalSupply_ = 1000000 * (10**decimals);

    mapping(address => uint256) balances;
    mapping(address => mapping(address => uint256)) allowed;

    
    constructor() {
        owner = msg.sender;
        balances[msg.sender] = totalSupply_;
    }

    function totalSupply() public override view returns(uint256) {
        return totalSupply_;
    }

    function balanceOf(address _owner) public override view returns(uint256) {
        return balances[_owner];
    }

    function transfer(address _receiver, uint256 _amount) public override returns(bool) {
        require(_amount <= balances[msg.sender]);
        balances[msg.sender] -=_amount;
        balances[_receiver] +=_amount;
        emit Transfer(msg.sender, _receiver, _amount);
        return true;
    }

    function approve(address _delegate, uint256 _amount) public override returns(bool) {
        allowed[msg.sender][_delegate] = _amount;
        emit Approval(msg.sender, _delegate, _amount);
        return true;
    }

    function allowance(address _owner, address _delegate) public override view returns(uint) {
        return allowed[_owner][_delegate];
    }

    function transferFrom(address _from, address _recipient, uint256 _amount) public override returns(bool) {
        require(_amount <= balances[_from]);
        require(_amount <= allowed[_from][msg.sender]);
        balances[_from] -=_amount;
        allowed[_from][msg.sender] -=_amount;
        balances[_recipient] +=_amount;
        emit Transfer(_from, _recipient, _amount);
        return true;
    }
   
}