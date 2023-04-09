pragma solidity ^0.5.12;

contract Owner {
    address public owner;
    bool public stopped = false;

    constructor() internal {
        owner = msg.sender;
    }

    modifier onlyOwner {
        require (msg.sender == owner);
        _;
    }

    function transferOwnership(address newOwner) external onlyOwner {
        require (newOwner != address(0));
        require (newOwner != owner);
        emit OwnerUpdate(owner, newOwner);
        owner = newOwner;
    }

    function toggleContractActive() onlyOwner public {
        stopped = !stopped;
    }

    modifier stopInEmergency {
        require(stopped == false);
        _;
    }

    modifier onlyInEmergency {
        require(stopped == true);
        _;
    }

    event OwnerUpdate(address _prevOwner, address _newOwner);
}

contract Mortal is Owner {
    function close() external onlyOwner {
        selfdestruct(msg.sender);
    }
}

/**
 * @title SafeMath
 * @dev Math operations with safety checks that throw on error
 */
library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a * b;
        assert(a == 0 || c / a == b);
        return c;
    }
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // assert(b > 0); // Solidity automatically throws when dividing by 0
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        return c;
    }
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }
}

contract Token is Owner, Mortal {
    using SafeMath for uint256;

    string public name; 
    string public symbol; 
    uint8 public decimals; 
    uint256 public totalSupply; 

  
    mapping (address => uint) public balances;
    mapping(address => mapping(address => uint)) approved;
 

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed accountOwner, address indexed spender, uint256 value);
    event Input(address indexed from,uint256 value);


    /**
    *
    * Fix for the ERC20 short address attack
    *
    * http://vessenes.com/the-erc20-short-address-attack-explained/
    */
    modifier onlyPayloadSize(uint256 size) {
        require(msg.data.length == size + 4);
        _;
    }
   
    function balanceOf(address sender) public view returns (uint256 balance){
        return balances[sender];
    }

    function transfer(address to, uint256 value) external stopInEmergency onlyPayloadSize(2 * 32) {
        _transfer(msg.sender, to, value);
    }

    function _transfer(address _from, address _to, uint _value) internal {
        require(_to != address(0));
        require(_from != _to);
        require(_value > 0);

        balances[_from] = balances[_from].sub(_value);
        balances[_to] = balances[_to].add(_value);
        emit Transfer(_from, _to, _value);
    }

    function approve(address spender, uint value) external returns (bool success) {
        approved[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);

        return true;
    }

    function allowance(address accountOwner, address spender) public view  returns (uint remaining) {
        return approved[accountOwner][spender];
    }

    function transferFrom(address from, address to, uint256 value) external stopInEmergency  returns (bool success) {
        require(value > 0);
        require(value <= approved[from][msg.sender]);
        require(value <= balances[from]);

        approved[from][msg.sender] = approved[from][msg.sender].sub(value);
        _transfer(from, to, value);
        return true;
    }
}

contract EcareToken is Owner, Token {
    constructor () public payable{
        name = "Ecare Coin";
        symbol = "ECARE";
        decimals = 18;
        owner = msg.sender;
        uint initialSupply = 100000000;
        totalSupply = initialSupply * 10 ** uint256(decimals);

        require (totalSupply >= initialSupply);
        balances[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }

}