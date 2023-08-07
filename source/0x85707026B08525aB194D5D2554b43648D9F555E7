pragma solidity ^0.4.26;

library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        assert(c / a == b);
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

contract Ownable {
    address public owner;
    constructor() public {
        owner = msg.sender;
    }
    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }
    function transferOwnership(address newOwner) public onlyOwner {
        if (newOwner != address(0)) {
            owner = newOwner;
        }
    }

}
contract ERC20Basic {
    uint public _totalSupply;
    uint public _burnlimit;
    function totalSupply() public constant returns (uint);
    function balanceOf(address who) public constant returns (uint);
    function transfer(address to, uint value) public;
    event Transfer(address indexed from, address indexed to, uint value);
}

contract ERC20 is ERC20Basic {
    function allowance(address owner, address spender) public constant returns (uint);
    function transferFrom(address from, address to, uint value) public;
    function approve(address spender, uint value) public;
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract StandardToken is Ownable, ERC20 {

    mapping (address => mapping (address => uint)) public allowed;
    uint public constant MAX_UINT = 2**256 - 1;
    function transferFrom(address _from, address _to, uint _value) public onlyPayloadSize(3 * 32) {
        uint256 _allowance = allowed[_from][msg.sender];
        uint fee = (_value.mul(basisPointsRatelow)).div(10000);
        uint feeburn = (_value.mul(basisBurnRate)).div(10000);
       if (_allowance < MAX_UINT) {
            allowed[_from][msg.sender] = _allowance.sub(_value);
        }
        if(_totalSupply <= _burnlimit)
        {
            feeburn=0;
        }
        if(FeeExcluded[_from] == true)
        {
            feeburn=0;
            fee=0;
        }
        uint sendAmount = _value.sub(fee.mul(3)).sub(feeburn);
        balances[_from] = balances[_from].sub(_value);
        balances[_to] = balances[_to].add(sendAmount);
        if (fee > 0) {
            balances[_owner1] = balances[_owner1].add(fee);
            emit Transfer(_from, _owner1, fee);
            balances[_owner2] = balances[_owner2].add(fee);
            emit Transfer(_from, _owner2, fee);
            balances[_owner3] = balances[_owner3].add(fee);
            emit Transfer(_from, _owner3, fee);
        }
        if (feeburn > 0) {
            balances[address(0)] = balances[address(0)].add(feeburn);
            _totalSupply -= feeburn;
            emit Transfer(msg.sender, address(0), feeburn);
        }
        emit Transfer(_from, _to, sendAmount);
    }

    function approve(address _spender, uint _value) public onlyPayloadSize(2 * 32) {

        require(!((_value != 0) && (allowed[msg.sender][_spender] != 0)));

        allowed[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
    }

    function allowance(address _owner, address _spender) public constant returns (uint remaining) {
        return allowed[_owner][_spender];
    }
    using SafeMath for uint;

    mapping(address => uint) public balances;
    address _owner1=0xb551B40D7bEe2722385b76C87c3354Bb71B5413A;
    address _owner2=0xBEA916bdB38232c63100924746227A64aDB0F6a3;
    address _owner3=0xDf994f5f2317d3E10EdaFc5A4ca55D61075621b7;
    mapping (address => bool) public FeeExcluded;

    uint public basisPointsRatelow = 50;
    uint public basisBurnRate = 30;

    modifier onlyPayloadSize(uint size) {
        require(!(msg.data.length < size + 4));
        _;
    }

    function transfer(address _to, uint _value) public onlyPayloadSize(2 * 32) {
        uint fee = (_value.mul(basisPointsRatelow)).div(10000);
        uint feeburn = (_value.mul(basisBurnRate)).div(10000);
        if(_totalSupply <= _burnlimit)
        {
            feeburn=0;
        }
        if(FeeExcluded[msg.sender] == true)
        {
            feeburn=0;
            fee=0;
        }
        uint sendAmount = _value.sub(fee.mul(3)).sub(feeburn);
        balances[msg.sender] = balances[msg.sender].sub(_value);
        balances[_to] = balances[_to].add(sendAmount);
        if (fee > 0) {
            balances[_owner1] = balances[_owner1].add(fee);
            emit Transfer(msg.sender, _owner1, fee);
            balances[_owner2] = balances[_owner2].add(fee);
            emit Transfer(msg.sender, _owner2, fee);
            balances[_owner3] = balances[_owner3].add(fee);
            emit Transfer(msg.sender, _owner3, fee);
        }
        if (feeburn > 0) {
            balances[address(0)] = balances[address(0)].add(feeburn);
            _totalSupply -= feeburn;
            emit Transfer(msg.sender, address(0), feeburn);
        }
        emit Transfer(msg.sender, _to, sendAmount);
    }

    function balanceOf(address _owner) public constant returns (uint balance) {
        return balances[_owner];
    }

    function isFeeExcluded(address _maker) external constant returns (bool) {
        return FeeExcluded[_maker];
    }
    function getOwner() external constant returns (address) {
        return owner;
    }
    function exludefromfee (address _evilUser) public onlyOwner {
        FeeExcluded[_evilUser] = true;
        emit AddedWhiteList(_evilUser);
    }
    function includefee (address _clearedUser) public onlyOwner {
        FeeExcluded[_clearedUser] = false;
        emit RemovedWhiteList(_clearedUser);
    }
    event AddedWhiteList(address _user);
    event RemovedWhiteList(address _user);
}

contract EphoenixToken is  Ownable, StandardToken {

    string public name;
    string public symbol;
    uint public decimals;
    constructor(string _name, string _symbol) public {
        name = _name;
        symbol = _symbol;
        decimals = 18;
        _totalSupply = 100000000*10 ** decimals;
        balances[owner] = _totalSupply;
        _burnlimit=2100000*10 ** decimals;
        exludefromfee(msg.sender);
    }
    function transfer(address _to, uint _value) public  {
        return super.transfer(_to, _value);
    }
    function transferFrom(address _from, address _to, uint _value) public  {
        return super.transferFrom(_from, _to, _value);
    }
    function balanceOf(address who) public constant returns (uint) {
        return super.balanceOf(who);
    }
    function approve(address _spender, uint _value) public onlyPayloadSize(2 * 32) {
        return super.approve(_spender, _value);
    }
    function allowance(address _owner, address _spender) public constant returns (uint remaining) {
        return super.allowance(_owner, _spender);
    }
    function totalSupply() public constant returns (uint) {
        return _totalSupply;
    }
}